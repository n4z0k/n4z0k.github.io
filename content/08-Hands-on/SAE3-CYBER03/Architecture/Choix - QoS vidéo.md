---
tags:
  - sae3
  - choix
  - qos
---
# Choix - QoS vidéo
---

> [[SAE3-CYBER03 - Accueil|Accueil]] · [[Plan d'adressage|Plan d'adressage]]

Méthode du TP4, en deux étapes.

## 1. Classification et marquage en entrée

Sur [[R25]] et [[R26]] (politique `MARQUAGE`, `service-policy input`). Le flux vidéo (TCP 8080 entre un serveur et les clients publics d'en face) est reconnu par l'ACL `VIDEO` et marqué DSCP **AF41** (34). Elle est appliquée sur Fe0/0 (côté LAN) et, pour R26, aussi sur E1/0.

## 2. Réservation en sortie

Politique `QOS-VIDEO`, `service-policy output`. La classe `DSCP-VIDEO` (`match dscp af41`) reçoit `bandwidth percent 30` ; le reste passe en `fair-queue`. Elle est appliquée sur toutes les interfaces traversées : Fe0/1 et Fe0/0 de R, E1/0 de [[R26]], Ge0/0/0 et Ge0/0/1 des ISR ([[ISR25]], [[ISR26]]).

Les ISR ne refont pas la classification : ils se fient au DSCP posé par R25 et R26, ce qui allège leur traitement. On utilise `bandwidth` et non `priority`, car le TP réserve `priority` (file à faible latence) à la voix.

## Équipements concernés

[[R25]], [[R26]], [[ISR25]], [[ISR26]] ; flux entre [[S25]] / [[S26]] et [[D25]] / [[D26]].

## Recette

[[01_TESTS_recette#6. QoS vidéo]]

## Voir aussi

[[Choix - ACL serveurs]] · [[Plan d'adressage]]
