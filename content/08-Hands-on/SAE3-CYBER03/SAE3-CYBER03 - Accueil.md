---
tags:
  - sae3
  - moc
  - reseau
---

# SAE3-CYBER03 - Accueil

**SAÉ réseau — BUT R&T 2e année, IUT Lannion.** Groupe 2B1 : ALI, DELUGEARD, GAILLARD, MALIN.

---
## Résumé du projet

Deux sites reliés par **OSPF** à travers un backbone fournisseur (zone 0) :

- **site m** = poste 25 (zone OSPF 25) : [[R25]], [[ISR25]], [[SW25]], serveur [[S25]], clients [[D25]] et [[G25]] ;
- **site n** = poste 26 (zone OSPF 26) : [[R26]], [[ISR26]], [[SW26-1]], [[SW26-2]], serveur [[S26]], clients [[D26]] et [[G26]].

Chaque site a un réseau de serveurs publics, un réseau de clients publics et un réseau de clients privés (192.168.m.0/24, jamais annoncé : NAT sur le routeur de site). Services : web, SSH, FTP, vidéo, DHCP, RADIUS (802.1X), avec ACL de protection des serveurs et QoS vidéo.

Ordre d'application : switchs → ISR → R25 / R26 → serveurs → clients → [[01_TESTS_recette|recette]] (détail dans [[00_LISEZMOI]]).

---
## Schémas

- Câblage : ![[SAE3-CYBER03_SchemaCablage_ALI_DELUGEARD_GAILLARD_MALIN.pdf]]
- Logique : ![[SAE3-CYBER03_SchemaLogique_ALI_DELUGEARD_GAILLARD_MALIN.pdf]]

Sources modifiables : [SAE3-CYBER03_SchemaCablage_ALI_DELUGEARD_GAILLARD_MALIN.drawio](Schemas/SAE3-CYBER03_SchemaCablage_ALI_DELUGEARD_GAILLARD_MALIN.drawio) · [SAE3-CYBER03_SchemaLogique_ALI_DELUGEARD_GAILLARD_MALIN.drawio](Schemas/SAE3-CYBER03_SchemaLogique_ALI_DELUGEARD_GAILLARD_MALIN.drawio)

---
## Routeurs

- [[R25]] — Cisco 2621XM, site m
- [[ISR25]] — ISR 4331, site m
- [[ISR26]] — ISR 4331, site n
- [[R26]] — Cisco 2621XM, site n

---
## Switchs

- [[SW25]] — Catalyst 2950, site m
- [[SW26-1]] — Catalyst 2950, site n
- [[SW26-2]] — Catalyst 2960, site n

---
## Machines

- [[S25]] — Debian 12, site m
- [[S26]] — Debian 12, site n
- [[D25]] — Debian 12, site m
- [[D26]] — Debian 12, site n
- [[G25]] — Debian 12, site m
- [[G26]] — Debian 12, site n

---
## Plan d'adressage et choix techniques

- [[Plan d'adressage]]
- [[Choix - OSPF multi-zones]]
- [[Choix - NAT et filtrage des adresses privées]]
- [[Choix - ACL serveurs]]
- [[Choix - QoS vidéo]]
- [[Choix - 802.1X RADIUS]]
- [[Choix - DHCP]]
- [[Identifiants]]
---
## Recette

- [[01_TESTS_recette]] (cahier de recette complet)
  - [[01_TESTS_recette#1. Couche 2 : VLAN et trunks|1. Couche 2 : VLAN et trunks]]
  - [[01_TESTS_recette#2. Routage OSPF|2. Routage OSPF]]
  - [[01_TESTS_recette#3. NAT et filtrage des adresses privées|3. NAT et filtrage des adresses privées]]
  - [[01_TESTS_recette#4. Services du serveur et ACL `PROTECT-SERVEURS`|4. Services du serveur et ACL]]
  - [[01_TESTS_recette#5. DHCP et 802.1X (RADIUS)|5. DHCP et 802.1X (RADIUS)]]
  - [[01_TESTS_recette#6. QoS vidéo|6. QoS vidéo]]
---
## Related 

- [[MOC_Hands-on|Hands-on]]

