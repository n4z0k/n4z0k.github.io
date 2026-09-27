---
author: Alfeze
created: 2026-08-29
---
# Basic Configuration

>Basic Cisco IOS switch configuration used to configure the hostname, secure local and remote access, encrypt passwords, configure a MOTD banner, and assign a management IP address to the switch.

---
## Initial Configuration

```text
Switch> enable
Switch# configure terminal

Switch(config)# hostname SW1
SW1(config)# no ip domain-lookup
SW1(config)# enable secret class

SW1(config)# line console 0
SW1(config-line)# password cisco
SW1(config-line)# login
SW1(config-line)# logging synchronous
SW1(config-line)# exit

SW1(config)# line vty 0 15
SW1(config-line)# logging synchronous
SW1(config-line)# password cisco
SW1(config-line)# login
SW1(config-line)# exit

SW1(config)# service password-encryption
SW1(config)# security password-length
SW1(config)# login block-for 120 attempts 3 within 60
SW1(config)# banner motd #Authorized Access Only#

SW1(config)# interface vlan 1
SW1(config-if)# ip address 192.168.1.20 255.255.255.0
SW1(config-if)# no shutdown
SW1(config-if)# exit

SW1(config)# ip default-gateway 192.168.1.1
SW1# copy running-config startup-config
```

## Gotchas

-  `>` = User EXEC mode.
* `#` = Privileged EXEC mode.
* `(config)#` = Global Configuration mode.
* `(config-line)#` = Line Configuration mode.
* `(config-if)#` = Interface Configuration mode.
* `no ip domain-lookup` prevents the switch from trying to resolve mistyped commands as hostnames through DNS.
* `logging synchronous` prevents console messages from disrupting commands being typed.
* `enable secret` protects access to Privileged EXEC mode.
* `service password-encryption` encrypts plain-text passwords in the configuration, but does not provide strong cryptographic protection.
* `security passwords min-length` enforces a minimum length for newly configured passwords; it does not modify existing passwords.
* `login block-for 120 attempts 3 within 60` blocks login attempts for **120 seconds** if **3 failed login attempts occur within 60 seconds**. (3 failed attempts within 60s : Login blocked for 120s)
* `login` is required for the configured console or VTY password to be requested.
* `no shutdown` administratively enables the SVI.
* An SVI can remain down even with `no shutdown` if the required Layer 2 conditions are not met.
* `ip default-gateway` is typically used on a Layer 2 switch.
* VLAN 1 is commonly used in labs; a dedicated management VLAN is preferred in production.
* The `running-config` must be saved to `startup-config` if the configuration should survive a reboot.

---
## Switch Port Configuration

>Cisco switch ports can be configured with specific speed and duplex settings. Auto-MDIX can automatically detect the required Ethernet cable connection type and adjust the interface accordingly.

### Configuration Speed - Duplex - Mdix

```text
S1# configure terminal

S1(config)# interface fastEthernet 0/1
S1(config-if)# description LINK_TO_S2
S1(config-if)# speed auto
S1(config-if)# duplex auto
S1(config-if)# mdix auto
S1(config-if)# no shutdown
S1(config-if)# end

S1# copy running-config startup-config
```

By default, Ethernet interfaces generally use **auto-negotiation** to determine the best speed and duplex settings supported by both devices.

Speed and duplex can also be configured manually :

```text
S1# configure terminal 

S1(config)# interface fastEthernet 0/1 
S1(config-if)# duplex full 
S1(config-if)# speed 100 
S1(config-if)# no shutdown 
S1(config-if)# end
```
### Verify Auto-MDIX

On supported IOS/platforms:

```
S1# show controllers ethernet-controller fastEthernet 0/1 phy
```

Can be used to inspect physical-layer information, including Auto-MDIX status.

---
## Configure Router Interfaces

```text
R1(config)# interface gigabitEthernet 0/0
R1(config-if)# description LAN 1
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# ipv6 address 2001:DB8:ACAD:1::1/64 
R1(config-if)# ipv6 address FE80::1 link-local
R1(config-if)# no shutdown
R1(config-if)# exit

R1(config)# interface gigabitEthernet 0/1
R1(config-if)# description LAN 2
R1(config-if)# ip address 192.168.2.1 255.255.255.0
R1(config-if)# ipv6 address 2001:DB8:ACAD:2::1/64
R1(config-if)# ipv6 address FE80::1 link-local
R1(config-if)# no shutdown
R1(config-if)# exit
```

---
## Loopback Interface

>A loopback interface is a **logical interface** internal to the router. It is not associated with a physical port and normally remains **up as long as the router is operational**.

Loopback interfaces are commonly used for **management, testing, routing protocols, and simulating networks** in labs.

```text
R1# configure terminal

R1(config)# interface loopback 0
R1(config-if)# ip address 10.0.0.1 255.255.255.255
R1(config-if)# exit
```

---
## Related

- [[MOC_Networking|Networking]]
