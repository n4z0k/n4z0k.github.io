---
tags:
  - sae3
  - choix
  - 802-1x
  - radius
---
# Choix - 802.1X RADIUS
---

> [[SAE3-CYBER03 - Accueil|Accueil]] · [[Plan d'adressage|Plan d'adressage]]

Contrôle d'accès des clients privés (méthode du TP5) : **802.1X** sur les ports 1 à 10 de [[SW25]] et [[SW26-2]], avec le RADIUS (FreeRADIUS) du serveur du site ([[S25]], [[S26]]), en EAP-MD5 comme au TP (PEAP est plus sûr si on veut aller plus loin).

- **Côté switch** : `aaa new-model`, `aaa authentication dot1x`, `aaa authorization network`, `dot1x system-auth-control`, puis `authentication port-control auto` et `dot1x pae authenticator` sur le 2960 ([[SW26-2]]). Le 2950 ([[SW25]]) utilise l'ancienne syntaxe (`radius-server host`, `dot1x port-control auto`).
- **Côté serveur** : le NAS est déclaré dans `clients.conf` (`nastype = cisco`), et l'utilisateur est ajouté **en début** de `/etc/freeradius/3.0/users`.
- **Côté client** : `/etc/wpa_supplicant/monradius.conf` sur [[G25]] / [[G26]].

## Adresse IP du NAS

Comme au TP5, elle n'est **pas** dans le VLAN des utilisateurs contrôlés (VLAN 192). Un poste non authentifié ne doit pas pouvoir joindre le NAS, et le NAS doit joindre le RADIUS sans dépendre de 802.1X.

- **Site m** : dans le VLAN 50, avec le serveur (équivalent du VLAN « radius » du TP) : SW25 = 50.50.25.250.
- **Site n** : le serveur est sur un autre switch ([[SW26-1]]), donc le NAS est dans le VLAN 150 (SW26-2 = 150.150.26.250) et la requête RADIUS est routée par [[R26]] (ligne RADIUS de l'ACL `PROTECT-SERVEURS`, voir [[Choix - ACL serveurs]]).

## Équipements concernés

[[SW25]], [[SW26-2]], [[SW26-1]], [[R26]], [[S25]], [[S26]], [[G25]], [[G26]]

## Recette

[[01_TESTS_recette#5. DHCP et 802.1X (RADIUS)]] · comptes : [[Identifiants]]

## Voir aussi

[[Choix - DHCP]] · [[Plan d'adressage]]
