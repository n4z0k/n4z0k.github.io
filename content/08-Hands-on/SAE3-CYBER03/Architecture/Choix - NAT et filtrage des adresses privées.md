---
tags:
  - sae3
  - choix
  - nat
  - acl
---
# Choix - NAT et filtrage des adresses privées
---

> [[SAE3-CYBER03 - Accueil|Accueil]] · [[Plan d'adressage|Plan d'adressage]]

## Aucune adresse privée vers les ISR

Trois mesures se complètent :

- le réseau 192.168.m.0/24 n'est pas déclaré dans OSPF ([[Choix - OSPF multi-zones]]), donc il n'est jamais annoncé ;
- les adresses privées sont traduites par NAT sur [[R25]] / [[R26]] ;
- l'ACL `FILTRE-PRIVE`, appliquée en sortie de Fe0/1, rejette tout paquet encore en adresse privée. Le NAT est appliqué avant l'ACL de sortie, donc seuls les paquets non traduits sont bloqués, par exemple ceux d'un client privé au-delà de .63.

## NAT dynamique avec un pool de 63 adresses, plutôt que du PAT

Le sujet demande que les 63 machines aux adresses les plus basses (192.168.m.1 à .63, ACL `0.0.0.63`) puissent accéder *simultanément* à l'autre site. Un pool de 63 adresses publiques donne une traduction 1:1 garantie pour chacune.

Le pool est pris dans le réseau d'interco 90.0.m.0/24, déjà annoncé en OSPF, ce qui évite toute route supplémentaire : le routeur R répond aux requêtes ARP de l'ISR ([[ISR25]] / [[ISR26]]) pour ces adresses. Un PAT sur l'adresse de Fe0/1 fonctionnerait aussi, mais ne réserverait pas une adresse par machine.

## Équipements concernés

[[R25]], [[R26]] (NAT et `FILTRE-PRIVE`), [[ISR25]], [[ISR26]] (ne voient jamais d'adresse privée), clients [[G25]], [[G26]].

## Recette

[[01_TESTS_recette#3. NAT et filtrage des adresses privées]]

## Voir aussi

[[Plan d'adressage]]
