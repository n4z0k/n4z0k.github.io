# SAE3-CYBER03 – Cahier de recette

---

## 1. Couche 2 : VLAN et trunks

| # | Où | Commande | Résultat attendu |
|---|---|---|---|
| 1.1 | SW25 | `show vlan brief` | VLAN 50 : Fa0/21-23 ; 150 : Fa0/11-20 ; 192 : Fa0/1-10 |
| 1.2 | SW25 | `show interfaces trunk` | Fa0/24 en trunk 802.1q, VLAN autorisés 50,150,192 |
| 1.3 | R25 | `show vlans` (ou `show ip interface brief`) | Sous-interfaces .50, .150, .192 up/up |

## 2. Routage OSPF

| # | Où | Commande | Résultat attendu |
|---|---|---|---|
| 2.1 | ISR25 | `show ip ospf neighbor` | 2 voisins FULL : 26.0.0.1 (ISR26, Ge0/0/0) et 25.0.0.2 (R25, Ge0/0/1) |
| 2.2 | ISR25 | `show ip ospf interface brief` | Ge0/0/0 en zone 0, Ge0/0/1 en zone 25 |
| 2.3 | R25 | `show ip route ospf` | Routes `O IA` vers 50.50.26.0, 150.150.26.0, 90.0.26.0 et 30.0.0.0 |
| 2.4 | ISR25 et ISR26 | `show ip route \| include 192.168` | **Aucune ligne** : pas de réseau privé dans le routage |
| 2.5 | R25 | `show ip protocols` | Interfaces passives : Fa0/0.50, Fa0/0.150, Fa0/0.192 |

## 3. NAT et filtrage des adresses privées

| # | Où | Commande | Résultat attendu |
|---|---|---|---|
| 3.1 | G25 (adresse .1 à .63) | `ping 150.150.26.1` | Réponses |
| 3.2 | R25 | `show ip nat translations` | 192.168.25.x ↔ 90.0.25.2 à .64 |
| 3.3 | ISR25 | `debug ip packet` (ou capture Wireshark sur le lien R25–ISR25) | Seules des adresses sources en 90.0.25.x, jamais en 192.168.x |
| 3.4 | G25 avec une adresse > .63 (pour le test : `ip addr flush dev eth0` puis `ip addr add 192.168.25.100/24 dev eth0` et `ip route add default via 192.168.25.254`) | `ping 150.150.26.1` | Échec ; les compteurs de `show access-lists FILTRE-PRIVE` augmentent sur la ligne `deny 192.168.0.0` |
| 3.5 | G25 | `ping 50.50.25.1` (serveur de son propre site) | Réponses, sans NAT (`show ip nat translations` ne change pas) |

## 4. Services du serveur et ACL `PROTECT-SERVEURS`

| # | Où | Commande | Résultat attendu |
|---|---|---|---|
| 4.1 | D26 (site d'en face) | `curl http://50.50.25.1` | Page « Serveur web S25 » |
| 4.2 | D26 | `ssh ftpuser@50.50.25.1` | Connexion |
| 4.3 | D26 | `ftp 50.50.25.1` puis `passive` et `ls` | Liste des fichiers (modes actif et passif) |
| 4.4 | D25 (même site) | Tests 4.1 à 4.3 | Identiques : accès depuis l'intérieur |
| 4.5 | D26 | `vlc http://50.50.25.1:8080` | Lecture de la vidéo |
| 4.6 | D25 ou G25 | `curl --max-time 5 http://50.50.25.1:8080` | **Échec** : la vidéo est réservée aux clients publics d'en face |
| 4.7 | D26 | `nc -zv 50.50.25.1 3306` (port non autorisé) | **Échec** ; la ligne `deny ip any any log` s'incrémente dans `show access-lists PROTECT-SERVEURS` |
| 4.8 | R25 | `show access-lists PROTECT-SERVEURS` | Compteurs (`matches`) sur chaque règle utilisée |

## 5. DHCP et 802.1X (RADIUS)

| # | Où | Commande | Résultat attendu |
|---|---|---|---|
| 5.1 | G25, wpa_supplicant arrêté | `pkill wpa_supplicant` puis `dhclient -v eth0` | Pas d'adresse : le port reste bloqué |
| 5.2 | SW25 | `show dot1x all` (2950) / `show authentication sessions` (2960) | Port Fa0/x non autorisé |
| 5.3 | G25 | `wpa_supplicant -B -D wired -c /etc/wpa_supplicant/monradius.conf -i eth0` puis `dhclient -v eth0` | Authentification réussie, puis bail 192.168.25.x |
| 5.4 | S25 | `systemctl stop freeradius` puis `freeradius -X` (mode debug du TP5) | `Access-Accept` pour g25 ; requêtes venant du NAS 50.50.25.250 |
| 5.5 | S25 | `cat /var/lib/dhcp/dhcpd.leases` | Bail attribué à G25 |
| 5.6 | G25 | Identifiants faux dans wpa_supplicant | `Access-Reject`, pas d'adresse IP |

## 6. QoS vidéo

| # | Où | Commande | Résultat attendu |
|---|---|---|---|
| 6.1 | R25, ISR25 | `show class-map` et `show policy-map` | Classes `VIDEO` et `DSCP-VIDEO`, politiques `MARQUAGE` et `QOS-VIDEO` |
| 6.2 | Wireshark entre R25 et ISR25 pendant le test 4.5 | Filtre `tcp.port == 8080` | Champ DSCP = **34 (AF41)** sur le flux vidéo, 0 sur les autres flux |
| 6.3 | R25, ISR25 | `show policy-map interface` | Paquets comptés dans `DSCP-VIDEO` (sortie) et `VIDEO` (entrée sur R25) |
| 6.4 | D26 | Lecture vidéo + transfert FTP volumineux ou ping continu (taille 1452, intervalle 0, comme au TP4) vers S25 | La vidéo continue sans coupure ; les compteurs de la classe `DSCP-VIDEO` augmentent |
