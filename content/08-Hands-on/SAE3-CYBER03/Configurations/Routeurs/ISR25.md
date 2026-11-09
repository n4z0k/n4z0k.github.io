---
type: routeur
modele: ISR 4331
site: m
adresses:
  - "Ge0/0/0 : 30.0.0.25/24 (backbone, zone 0)"
  - "Ge0/0/1 : 90.0.25.254/24 (interco R25, zone 25)"
tags:
  - sae3
  - cisco
  - site-m
---
# ISR25
---

> Routeur · ISR 4331 · site m · [[SAE3-CYBER03 - Accueil|Accueil]]

## Rôle

Routeur d'accès au fournisseur pour le site m. **ABR OSPF** : Ge0/0/0 en zone 0 (backbone 30.0.0.0/24), Ge0/0/1 en zone 25. Il applique la politique de sortie `QOS-VIDEO` (réservation 30 % pour DSCP AF41) mais ne reclasse pas le trafic : il se fie au marquage posé par [[R25]].

## Adresses

- Ge0/0/0 : 30.0.0.25/24 (backbone, zone 0)
- Ge0/0/1 : 90.0.25.254/24 (interco R25, zone 25)

## Voisins

- [[R25]] — Ge0/0/1 ↔ Fe0/1 du R25 (interco 90.0.25.0/24)
- [[ISR26]] — Ge0/0/0 ↔ Ge0/0/0 de l'ISR26 (backbone OSPF zone 0, 30.0.0.0/24)

## Choix techniques liés

- [[Choix - OSPF multi-zones]]
- [[Choix - NAT et filtrage des adresses privées]]
- [[Choix - QoS vidéo]]

## Configuration (avec prompt)

```cisco
Router>enable
Router#configure terminal
Router(config)#hostname ISR25
ISR25(config)#no ip domain-lookup
ISR25(config)#service password-encryption
ISR25(config)#enable secret Sae3Cyber!
ISR25(config)#username admin privilege 15 secret Sae3Cyber!
ISR25(config)#banner motd #Acces reserve - SAE3-CYBER03 - ISR25#
ISR25(config)#ip domain-name sae3cyber03.iut
ISR25(config)#ip cef
!
! ---------- QoS (methode du TP4) : etape 2 - politique de sortie ----------
! Les flux video sont reconnus par leur marquage DSCP AF41 (34), pose a l'entree du reseau
ISR25(config)#class-map match-all DSCP-VIDEO
ISR25(config-cmap)#match dscp af41
ISR25(config-cmap)#exit
ISR25(config)#policy-map QOS-VIDEO
ISR25(config-pmap)#class DSCP-VIDEO
ISR25(config-pmap-c)#bandwidth percent 30
ISR25(config-pmap-c)#exit
ISR25(config-pmap)#class class-default
ISR25(config-pmap-c)#fair-queue
ISR25(config-pmap-c)#exit
ISR25(config-pmap)#exit
!
! ---------- Interfaces ----------
ISR25(config)#interface GigabitEthernet0/0/0
ISR25(config-if)#description Backbone OSPF zone 0 - 30.0.0.0/24
ISR25(config-if)#ip address 30.0.0.25 255.255.255.0
ISR25(config-if)#service-policy output QOS-VIDEO
ISR25(config-if)#no shutdown
ISR25(config-if)#exit
ISR25(config)#interface GigabitEthernet0/0/1
ISR25(config-if)#description Interco vers R25 Fe0/1 - 90.0.25.0/24
ISR25(config-if)#ip address 90.0.25.254 255.255.255.0
ISR25(config-if)#service-policy output QOS-VIDEO
ISR25(config-if)#no shutdown
ISR25(config-if)#exit
!
! ---------- OSPF : ABR entre la zone 0 et la zone 25 ----------
ISR25(config)#router ospf 1
ISR25(config-router)#router-id 25.0.0.1
ISR25(config-router)#network 30.0.0.0 0.0.0.255 area 0
ISR25(config-router)#network 90.0.25.0 0.0.0.255 area 25
ISR25(config-router)#exit
!
! ---------- Acces console / SSH ----------
ISR25(config)#crypto key generate rsa general-keys modulus 1024
ISR25(config)#ip ssh version 2
ISR25(config)#line console 0
ISR25(config-line)#logging synchronous
ISR25(config-line)#login local
ISR25(config-line)#exit
ISR25(config)#line vty 0 4
ISR25(config-line)#login local
ISR25(config-line)#transport input ssh
ISR25(config-line)#exit
ISR25(config)#end
ISR25#write memory
```

## Fichier à copier-coller

- [ISR25_a_coller.txt](ISR25_a_coller.txt) — à coller tel quel dans la console
- [ISR25_avec_prompt.txt](ISR25_avec_prompt.txt) — version avec prompt (compte rendu)
