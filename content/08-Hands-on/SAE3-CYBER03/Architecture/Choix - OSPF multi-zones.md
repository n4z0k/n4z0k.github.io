---
tags:
  - sae3
  - choix
  - ospf
---
# Choix - OSPF multi-zones
---

> [[SAE3-CYBER03 - Accueil|Accueil]] · [[Plan d'adressage|Plan d'adressage]]

Le sujet impose d'être rattaché à la zone 0 du fournisseur *et* d'avoir une zone propre portant le numéro du site.

- Les ISR ([[ISR25]], [[ISR26]]) sont donc **ABR** : Ge0/0/0 en zone 0, Ge0/0/1 en zone 25 (ou 26).
- Les routeurs de site ([[R25]], [[R26]]) sont entièrement en zone 25 (ou 26).
- Les interfaces LAN sont en `passive-interface` : leurs réseaux sont annoncés, mais aucun hello OSPF n'est envoyé vers les machines.
- Le réseau privé 192.168.m.0/24 n'est volontairement **pas** déclaré (voir [[Choix - NAT et filtrage des adresses privées]]).

## Équipements concernés

[[R25]], [[ISR25]], [[ISR26]], [[R26]]

## Recette

[[01_TESTS_recette#2. Routage OSPF]]

## Voir aussi

[[Plan d'adressage]]
