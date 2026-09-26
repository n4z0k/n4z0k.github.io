---
author: Alfeze
created: 2026-08-29
---
# Router-on-a-Stick — Inter-VLAN Routing

## Topology

> Router-on-a-Stick allows **inter-VLAN routing** using a single physical router interface divided into multiple subinterfaces.

![[Pasted image 20260829184819.png|388]]

![[Pasted image 20260829184941.png|398]]

- **VLAN 10** → PC1
- **VLAN 20** → PC2
- **VLAN 99** → Management
- S1 ↔ R1 = Trunk
- S1 ↔ S2 = Trunk

---
## Configure S1

### Create the VLANs

```
S1# configure terminal

S1(config)# vlan 10
S1(config-vlan)# name LAN10
S1(config-vlan)# exit

S1(config)# vlan 20
S1(config-vlan)# name LAN20
S1(config-vlan)# exit

S1(config)# vlan 99
S1(config-vlan)# name Management
S1(config-vlan)# exit
```

### Configure the Management SVI

```
S1(config)# interface vlan 99
S1(config-if)# ip address 192.168.99.2 255.255.255.0
S1(config-if)# no shutdown
S1(config-if)# exit

S1(config)# ip default-gateway 192.168.99.1
```

### Configure PC1 Access Port — VLAN 10

PC1 is connected to **F0/6**.

```
S1(config)# interface f0/6
S1(config-if)# switchport mode access
S1(config-if)# switchport access vlan 10
S1(config-if)# no shutdown
S1(config-if)# exit
```
### Configure the Trunk to R1

R1 is connected to **F0/5**.

```
S1(config)# interface f0/5
S1(config-if)# switchport mode trunk
S1(config-if)# switchport trunk allowed vlan 10,20,99
S1(config-if)# no shutdown
S1(config-if)# exit
```

### Configure the Trunk to S2

S2 is connected to **F0/1**.

```
S1(config)# interface f0/1
S1(config-if)# switchport mode trunk
S1(config-if)# switchport trunk allowed vlan 10,20,99
S1(config-if)# no shutdown
S1(config-if)# exit
```

Save the configuration:

```
S1(config)# end
S1# copy running-config startup-config
```

---
## Configure S2

> S2 uses the same VLAN and trunk configuration as S1.  
> The main differences are the **management IP address** and the **access port/VLAN**.

### Create the VLANs

```
S2# configure terminal

S2(config)# vlan 10
S2(config-vlan)# name LAN10
S2(config-vlan)# exit

S2(config)# vlan 20
S2(config-vlan)# name LAN20
S2(config-vlan)# exit

S2(config)# vlan 99
S2(config-vlan)# name Management
S2(config-vlan)# exit
```

### Configure the Management SVI

```
S2(config)# interface vlan 99
S2(config-if)# ip address 192.168.99.3 255.255.255.0
S2(config-if)# no shutdown
S2(config-if)# exit

S2(config)# ip default-gateway 192.168.99.1
```

### Configure PC2 Access Port — VLAN 20

PC2 is connected to **F0/18**.

```
S2(config)# interface f0/18
S2(config-if)# switchport mode access
S2(config-if)# switchport access vlan 20
S2(config-if)# no shutdown
S2(config-if)# exit
```

### Configure the Trunk to S1

```
S2(config)# interface f0/1
S2(config-if)# switchport mode trunk
S2(config-if)# switchport trunk allowed vlan 10,20,99
S2(config-if)# no shutdown
```

---
## Configure R1 — Router-on-a-Stick

Each VLAN requires a **subinterface** on `G0/0/1`.

### Enable the Physical Interface

```
R1(config)# interface g0/0/1
R1(config-if)# no shutdown
R1(config-if)# end
```

> [!important]  
> The physical interface `G0/0/1` does **not** receive an IP address.  
> IP addresses are configured on the subinterfaces.
> 
> Each subinterface acts as the **default gateway for its VLAN**.

### VLAN 10

```
R1(config)# interface g0/0/1.10
R1(config-subif)# encapsulation dot1Q 10
R1(config-subif)# ip address 192.168.10.1 255.255.255.0
R1(config-subif)# exit
```

### VLAN 20

```
R1(config)# interface g0/0/1.20
R1(config-subif)# encapsulation dot1Q 20
R1(config-subif)# ip address 192.168.20.1 255.255.255.0
R1(config-subif)# exit
```

### VLAN 99

```
R1(config)# interface g0/0/1.99
R1(config-subif)# encapsulation dot1Q 99
R1(config-subif)# ip address 192.168.99.1 255.255.255.0
R1(config-subif)# exit
```

---
## End Device Addressing

### PC1 — VLAN 10

```
IP Address:      192.168.10.10
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.10.1
```

### PC2 — VLAN 20

```
IP Address:      192.168.20.10
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.20.1
```

---
## Verification

### Router

```
R1# show ip interface brief
R1# show ip route
```

The routing table should contain the VLAN networks as **directly connected networks**.

### Switches

```
S1# show vlan brief
S1# show interfaces trunk
```

### Test Inter-VLAN Connectivity

From PC1:

```
PC1> ping 192.168.20.10
```

> [!tip]  
> If inter-VLAN routing does not work, check:  
> **VLANs → access ports → trunks → allowed VLANs → subinterfaces → `encapsulation dot1Q` → IP addresses → default gateways → `no shutdown`**.

---

> [!TIP] **Scalability** - **Layer 3 switches**
> **Router-on-a-Stick** is mainly suited for **small to medium-sized networks**.  
For larger infrastructures, **Layer 3 switches** are preferred for Inter-VLAN Routing because routing can be performed directly by the switch.

On a Layer 3 switch, a physical switch port can be converted into a **routed port**:

```
SW-L3(config)# interface g1/0/1
SW-L3(config-if)# no switchport
SW-L3(config-if)# ip address 10.10.10.1 255.255.255.252
SW-L3(config-if)# no shutdown
```

Then enable Layer 3 routing:

```
SW-L3(config)# ip routing
```

`no switchport` → converts a Layer 2 switch port into a **Layer 3 routed port**, allowing an IP address to be assigned directly to it.

---
## Related

- [[MOC_Networking|Networking]]
- [[VLAN]]
