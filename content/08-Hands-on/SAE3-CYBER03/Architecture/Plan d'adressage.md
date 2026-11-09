---
tags:
  - sae3
  - adressage
---
# Plan d'adressage
---

> [[SAE3-CYBER03 - Accueil|Accueil]] · [[Plan d'adressage|Plan d'adressage]]

## Réseaux

| Réseau | Site m (25) | Site n (26) | Passerelle | Annoncé en OSPF |
|---|---|---|---|---|
| Serveurs publics (VLAN 50 sur le site m, sans VLAN sur le site n) | 50.50.25.0/24 | 50.50.26.0/24 | .254 | oui, zone 25 / 26 |
| Clients publics (VLAN 150) | 150.150.25.0/24 | 150.150.26.0/24 | .254 | oui, zone 25 / 26 |
| Clients privés (VLAN 192) | 192.168.25.0/24 | 192.168.26.0/24 | .254 | **non** |
| Interco R ↔ ISR | 90.0.25.0/24 (R .1, ISR .254) | 90.0.26.0/24 (R .1, ISR .254) | – | oui, zone 25 / 26 |
| Backbone fournisseur | 30.0.0.25 | 30.0.0.26 | – | oui, zone 0 |

Passerelles : [[R25]] (site m) et [[R26]] (site n). Interco : [[ISR25]] ↔ [[R25]] et [[ISR26]] ↔ [[R26]]. Backbone : [[ISR25]] ↔ [[ISR26]].

## Machines et adresses

| Machine | Adresse | Équipement |
|---|---|---|
| S25 / S26 | 50.50.25.1 / 50.50.26.1 | [[S25]], [[S26]] |
| D25 / D26 | 150.150.25.1 / 150.150.26.1 | [[D25]], [[D26]] |
| G25 / G26 | DHCP (192.168.m.1 à .128) | [[G25]], [[G26]] |
| SW25 (NAS RADIUS) | 50.50.25.250 (VLAN 50, même réseau que le serveur RADIUS S25) | [[SW25]] |
| SW26-2 (NAS RADIUS) | 150.150.26.250 (VLAN 150) | [[SW26-2]] |
| SW26-1 | 50.50.26.250 | [[SW26-1]] |
| Pool NAT | 90.0.25.2 – 90.0.25.64 / 90.0.26.2 – 90.0.26.64 | [[R25]], [[R26]] |

## Voir aussi

- [[Choix - OSPF multi-zones]]
- [[Choix - NAT et filtrage des adresses privées]]
- [[Choix - DHCP]]
- [[Choix - 802.1X RADIUS]]
- [[Identifiants]]
