---
tags:
  - sae3
  - choix
  - acl
---
# Choix - ACL serveurs
---

> [[SAE3-CYBER03 - Accueil|Accueil]] · [[Plan d'adressage|Plan d'adressage]]

`PROTECT-SERVEURS` est une ACL étendue placée en **sortie de l'interface des serveurs** (Fe0/0.50 sur [[R25]], E1/0 sur [[R26]]). Seul le trafic destiné aux serveurs ([[S25]], [[S26]]) est examiné, ce qui limite le traitement comme le demande le sujet.

Elle laisse passer uniquement les flux nécessaires :

- web (80, 443), SSH (22) et FTP (21, 20, et la plage passive 40000 à 40100) ;
- la vidéo (8080), uniquement depuis les clients publics de l'autre site ([[D25]], [[D26]]) — voir [[Choix - QoS vidéo]] ;
- RADIUS (1812, 1813) depuis le switch d'accès ([[Choix - 802.1X RADIUS]]), et DHCP ([[Choix - DHCP]]) ;
- l'ICMP pour les tests.

Les règles les plus utilisées sont placées en tête de liste ; `deny ip any any log` termine l'ACL.

## Équipements concernés

[[R25]], [[R26]], [[S25]], [[S26]], [[SW26-1]]

## Recette

[[01_TESTS_recette#4. Services du serveur et ACL `PROTECT-SERVEURS`]]

## Voir aussi

[[Plan d'adressage]]
