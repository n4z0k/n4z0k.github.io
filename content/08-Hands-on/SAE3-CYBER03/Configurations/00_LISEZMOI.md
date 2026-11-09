# SAE3-CYBER03 – Configurations des équipements

Groupe 2B1 – ALI, DELUGEARD, GAILLARD, MALIN

---

## Contenu du dossier

| Dossier | Fichiers | Usage |
|---|---|---|
| `Routeurs/` | `R25`, `ISR25`, `ISR26`, `R26` | `_a_coller.txt` à copier-coller dans la console ; `_avec_prompt.txt` pour le compte rendu (commandes avec le prompt, exigé par le sujet) |
| `Switchs/` | `SW25`, `SW26-1`, `SW26-2` | Idem |
| `Machines/` | `S25`, `S26`, `D25`, `D26`, `G25`, `G26` | Configuration à chaud Debian (commandes `ip`, `systemctl`…) : `_a_coller.txt` à coller dans le terminal root ; `_avec_prompt.txt` avec le prompt `root@D25:~#` pour le compte rendu |
| `01_TESTS_recette.md` | | Cahier de recette : commandes de vérification et résultats attendus |

## Ordre d'application

1. **Switchs** (SW25, SW26-1, SW26-2) : les VLAN et trunks doivent exister avant les routeurs.
2. **ISR25 et ISR26** : backbone OSPF zone 0.
3. **R25 et R26**.
4. **Serveurs S25 et S26** 
5. **Clients** D25, D26, puis G25, G26 (besoin du RADIUS et du DHCP de leur serveur).
6. Dérouler `01_TESTS_recette.md`.

## Plan d'adressage

| Réseau | Site m (25) | Site n (26) | Passerelle | Annoncé en OSPF |
|---|---|---|---|---|
| Serveurs publics (VLAN 50 sur le site m, sans VLAN sur le site n) | 50.50.25.0/24 | 50.50.26.0/24 | .254 | oui, zone 25 / 26 |
| Clients publics (VLAN 150) | 150.150.25.0/24 | 150.150.26.0/24 | .254 | oui, zone 25 / 26 |
| Clients privés (VLAN 192) | 192.168.25.0/24 | 192.168.26.0/24 | .254 | **non** |
| Interco R ↔ ISR | 90.0.25.0/24 (R .1, ISR .254) | 90.0.26.0/24 (R .1, ISR .254) | – | oui, zone 25 / 26 |
| Backbone fournisseur | 30.0.0.25 | 30.0.0.26 | – | oui, zone 0 |

| Machine | Adresse |
|---|---|
| S25 / S26 | 50.50.25.1 / 50.50.26.1 |
| D25 / D26 | 150.150.25.1 / 150.150.26.1 |
| G25 / G26 | DHCP (192.168.m.1 à .128) |
| SW25 (NAS RADIUS) | 50.50.25.250 (VLAN 50, même réseau que le serveur RADIUS S25) |
| SW26-2 (NAS RADIUS) | 150.150.26.250 (VLAN 150) |
| SW26-1 | 50.50.26.250 |
| Pool NAT | 90.0.25.2 – 90.0.25.64 / 90.0.26.2 – 90.0.26.64 |

## Choix techniques

**OSPF multi-zones.** Le sujet impose d'être rattaché à la zone 0 du fournisseur *et* d'avoir une zone propre portant le numéro du site. Les ISR sont donc ABR : Ge0/0/0 en zone 0, Ge0/0/1 en zone 25 (ou 26). Les R26xx sont entièrement en zone 25 (ou 26). Les interfaces LAN sont en `passive-interface` : leurs réseaux sont annoncés, mais aucun hello OSPF n'est envoyé vers les machines.

**Aucune adresse privée vers les ISR.** Trois mesures se complètent :
- le réseau 192.168.m.0/24 n'est pas déclaré dans OSPF, donc il n'est jamais annoncé ;
- les adresses privées sont traduites par NAT sur R25 / R26 ;
- l'ACL `FILTRE-PRIVE`, appliquée en sortie de Fe0/1, rejette tout paquet encore en adresse privée. Le NAT est appliqué avant l'ACL de sortie, donc seuls les paquets non traduits sont bloqués, par exemple ceux d'un client privé au-delà de .63.

**NAT dynamique avec un pool de 63 adresses, plutôt que du PAT.** Le sujet demande que les 63 machines aux adresses les plus basses (192.168.m.1 à .63, ACL `0.0.0.63`) puissent accéder *simultanément* à l'autre site. Un pool de 63 adresses publiques donne une traduction 1:1 garantie pour chacune. Le pool est pris dans le réseau d'interco 90.0.m.0/24, déjà annoncé en OSPF, ce qui évite toute route supplémentaire : R répond aux requêtes ARP de l'ISR pour ces adresses. Un PAT sur l'adresse de Fe0/1 fonctionnerait aussi, mais ne réserverait pas une adresse par machine.

**ACL `PROTECT-SERVEURS`.** C'est une ACL étendue placée en **sortie de l'interface des serveurs** (Fe0/0.50 sur R25, E1/0 sur R26). Seul le trafic destiné aux serveurs est examiné, ce qui limite le traitement comme le demande le sujet. Elle laisse passer uniquement les flux nécessaires :
- web (80, 443), SSH (22) et FTP (21, 20, et la plage passive 40000 à 40100) ;
- la vidéo (8080), uniquement depuis les clients publics de l'autre site ;
- RADIUS (1812, 1813) depuis le switch d'accès, et DHCP ;
- l'ICMP pour les tests.

Les règles les plus utilisées sont placées en tête de liste.

**QoS vidéo (méthode du TP4).** Elle se fait en deux étapes :
1. **Classification et marquage en entrée** sur R25 et R26 (politique `MARQUAGE`, `service-policy input`). Le flux vidéo (TCP 8080 entre un serveur et les clients publics d'en face) est reconnu par l'ACL `VIDEO` et marqué DSCP **AF41** (34). Elle est appliquée sur Fe0/0 (côté LAN) et sur E1/0 pour R26.
2. **Réservation en sortie** (politique `QOS-VIDEO`, `service-policy output`). La classe `DSCP-VIDEO` (`match dscp af41`) reçoit `bandwidth percent 30` ; le reste passe en `fair-queue`. Elle est appliquée sur toutes les interfaces traversées : Fe0/1 et Fe0/0 de R, E1/0 de R26, Ge0/0/0 et Ge0/0/1 des ISR.

Les ISR ne refont pas la classification : ils se fient au DSCP posé par R25 et R26, ce qui allège leur traitement. On utilise `bandwidth` et non `priority`, car le TP réserve `priority` (file à faible latence) à la voix.

**Contrôle d'accès des clients privés (méthode du TP5).** Il passe par 802.1X sur les ports 1 à 10 de SW25 et SW26-2, avec le RADIUS (FreeRADIUS) du serveur du site, en EAP-MD5 comme au TP (PEAP est plus sûr si vous voulez aller plus loin).
- Côté switch : `aaa new-model`, `aaa authentication dot1x`, `aaa authorization network`, `dot1x system-auth-control`, puis `authentication port-control auto` et `dot1x pae authenticator` sur le 2960. Le 2950 utilise l'ancienne syntaxe (`radius-server host`, `dot1x port-control auto`).
- Côté serveur : le NAS est déclaré dans `clients.conf` (`nastype = cisco`), et l'utilisateur est ajouté **en début** de `/etc/freeradius/3.0/users`.
- Côté client : `/etc/wpa_supplicant/monradius.conf`.
- **Adresse IP du NAS** : comme au TP5, elle n'est **pas** dans le VLAN des utilisateurs contrôlés (VLAN 192). Un poste non authentifié ne doit pas pouvoir joindre le NAS, et le NAS doit joindre le RADIUS sans dépendre de 802.1X. Sur le site m, elle est dans le VLAN 50, avec le serveur (équivalent du VLAN « radius » du TP). Sur le site n, le serveur est sur un autre switch (SW26-1) : elle est donc dans le VLAN 150, et la requête RADIUS est routée par R26 (ligne RADIUS de l'ACL `PROTECT-SERVEURS` de R26).

**DHCP.** Le serveur DHCP tourne sur le serveur du site (isc-dhcp-server). R25 / R26 relaient les requêtes avec `ip helper-address`. La plage .1 à .128 correspond aux 128 machines maximum demandées.

## Identifiants

| Élément | Valeur |
|---|---|
| `enable secret` et utilisateur `admin` (tous les Cisco) | `Sae3Cyber!` |
| Clé RADIUS (switch ↔ serveur) | `RadiusSae3` |
| Comptes 802.1X | `g25` / `G25pass!`, `g26` / `G26pass!` |
| Compte FTP | `ftpuser` / `Ftp25pass!` (S25), `Ftp26pass!` (S26) |
