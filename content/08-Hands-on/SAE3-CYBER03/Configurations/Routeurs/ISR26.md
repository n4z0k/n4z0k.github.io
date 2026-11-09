---
type: routeur
modele: ISR 4331
site: n
adresses:
  - "Ge0/0/0 : 30.0.0.26/24 (backbone, zone 0)"
  - "Ge0/0/1 : 90.0.26.254/24 (interco R26, zone 26)"
tags:
  - sae3
  - cisco
  - site-n
---
# ISR26
---

> Routeur · ISR 4331 · site n · [[SAE3-CYBER03 - Accueil|Accueil]]

## Rôle

Routeur d'accès au fournisseur pour le site n. **ABR OSPF** : Ge0/0/0 en zone 0 (backbone 30.0.0.0/24), Ge0/0/1 en zone 26. Il applique la politique de sortie `QOS-VIDEO` (réservation 30 % pour DSCP AF41) en se fiant au marquage posé par [[R26]].

## Adresses

- Ge0/0/0 : 30.0.0.26/24 (backbone, zone 0)
- Ge0/0/1 : 90.0.26.254/24 (interco R26, zone 26)

## Voisins

- [[R26]] — Ge0/0/1 ↔ Fe0/1 du R26 (interco 90.0.26.0/24)
- [[ISR25]] — Ge0/0/0 ↔ Ge0/0/0 de l'ISR25 (backbone OSPF zone 0, 30.0.0.0/24)

## Choix techniques liés

- [[Choix - OSPF multi-zones]]
- [[Choix - NAT et filtrage des adresses privées]]
- [[Choix - QoS vidéo]]

## Configuration (avec prompt)

```cisco
Router>enable
Router#configure terminal
Router(config)#hostname ISR26
ISR26(config)#no ip domain-lookup
ISR26(config)#service password-encryption
ISR26(config)#enable secret Sae3Cyber!
ISR26(config)#username admin privilege 15 secret Sae3Cyber!
ISR26(config)#banner motd #Acces reserve - SAE3-CYBER03 - ISR26#
ISR26(config)#ip domain-name sae3cyber03.iut
ISR26(config)#ip cef
!
! ---------- QoS (methode du TP4) : etape 2 - politique de sortie ----------
! Les flux video sont reconnus par leur marquage DSCP AF41 (34), pose a l'entree du reseau
ISR26(config)#class-map match-all DSCP-VIDEO
ISR26(config-cmap)#match dscp af41
ISR26(config-cmap)#exit
ISR26(config)#policy-map QOS-VIDEO
ISR26(config-pmap)#class DSCP-VIDEO
ISR26(config-pmap-c)#bandwidth percent 30
ISR26(config-pmap-c)#exit
ISR26(config-pmap)#class class-default
ISR26(config-pmap-c)#fair-queue
ISR26(config-pmap-c)#exit
ISR26(config-pmap)#exit
!
! ---------- Interfaces ----------
ISR26(config)#interface GigabitEthernet0/0/0
ISR26(config-if)#description Backbone OSPF zone 0 - 30.0.0.0/24
ISR26(config-if)#ip address 30.0.0.26 255.255.255.0
ISR26(config-if)#service-policy output QOS-VIDEO
ISR26(config-if)#no shutdown
ISR26(config-if)#exit
ISR26(config)#interface GigabitEthernet0/0/1
ISR26(config-if)#description Interco vers R26 Fe0/1 - 90.0.26.0/24
ISR26(config-if)#ip address 90.0.26.254 255.255.255.0
ISR26(config-if)#service-policy output QOS-VIDEO
ISR26(config-if)#no shutdown
ISR26(config-if)#exit
!
! ---------- OSPF : ABR entre la zone 0 et la zone 26 ----------
ISR26(config)#router ospf 1
ISR26(config-router)#router-id 26.0.0.1
ISR26(config-router)#network 30.0.0.0 0.0.0.255 area 0
ISR26(config-router)#network 90.0.26.0 0.0.0.255 area 26
ISR26(config-router)#exit
!
! ---------- Acces console / SSH ----------
ISR26(config)#crypto key generate rsa general-keys modulus 1024
ISR26(config)#ip ssh version 2
ISR26(config)#line console 0
ISR26(config-line)#logging synchronous
ISR26(config-line)#login local
ISR26(config-line)#exit
ISR26(config)#line vty 0 4
ISR26(config-line)#login local
ISR26(config-line)#transport input ssh
ISR26(config-line)#exit
ISR26(config)#end
ISR26#write memory
```

## Fichier à copier-coller

- [ISR26_a_coller.txt](ISR26_a_coller.txt) — à coller tel quel dans la console
- [ISR26_avec_prompt.txt](ISR26_avec_prompt.txt) — version avec prompt (compte rendu)
