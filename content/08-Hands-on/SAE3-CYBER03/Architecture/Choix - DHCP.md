---
tags:
  - sae3
  - choix
  - dhcp
---
# Choix - DHCP
---

> [[SAE3-CYBER03 - Accueil|Accueil]] · [[Plan d'adressage|Plan d'adressage]]

Le serveur DHCP tourne sur le serveur du site (isc-dhcp-server : [[S25]], [[S26]]). Les routeurs [[R25]] / [[R26]] relaient les requêtes des clients privés avec `ip helper-address` sur la sous-interface VLAN 192. La plage .1 à .128 correspond aux 128 machines maximum demandées.

Les clients [[G25]] / [[G26]] n'obtiennent une adresse qu'après authentification 802.1X ([[Choix - 802.1X RADIUS]]). L'ACL `PROTECT-SERVEURS` laisse passer le DHCP relayé (UDP 67) vers le serveur ([[Choix - ACL serveurs]]).

## Équipements concernés

[[S25]], [[S26]], [[R25]], [[R26]], [[G25]], [[G26]]

## Recette

[[01_TESTS_recette#5. DHCP et 802.1X (RADIUS)]]

## Voir aussi

[[Plan d'adressage]]
