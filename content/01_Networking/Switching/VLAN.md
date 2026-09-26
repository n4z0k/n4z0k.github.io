---
author: Alfeze
created: 2026-08-29
---

# VLAN

> A **VLAN (Virtual LAN)** logically divides a Layer 2 switch into separate **broadcast domains**. Devices in different VLANs require a Layer 3 device to communicate with each other.

---
## Create VLANs

```text
S1# configure terminal

S1(config)# vlan 10
S1(config-vlan)# name STAFF
S1(config-vlan)# exit

S1(config)# vlan 20
S1(config-vlan)# name STUDENT
S1(config-vlan)# exit

S1(config)# vlan 30
S1(config-vlan)# name GUEST
S1(config-vlan)# exit
```

>The `name` command is optional. If no name is configured, IOS assigns a default name such as `VLAN0020`.

---
## Assign an Access Port to a VLAN

An **access port belongs to one data VLAN** and is typically used to connect an end device such as a PC.

```text
S1(config)# interface fastEthernet 0/18
S1(config-if)# switchport mode access
S1(config-if)# switchport access vlan 20
S1(config-if)# no shutdown
S1(config-if)# exit
```

---
## Configure Multiple Access Ports

```text
S1(config)# interface range fastEthernet 0/10 - 18
S1(config-if-range)# switchport mode access
S1(config-if-range)# switchport access vlan 20
S1(config-if-range)# no shutdown
S1(config-if-range)# exit
```

---
## Voice VLAN

A switch port can carry a **data VLAN** for a PC and a separate **voice VLAN** for an IP phone.

![[Pasted image 20260829175642.png|403]]

```text
S3# configure terminal

S3(config)# vlan 20
S3(config-vlan)# name STUDENT
S3(config-vlan)# exit

S3(config)# vlan 150
S3(config-vlan)# name VOICE
S3(config-vlan)# exit

S3(config)# interface fastEthernet 0/18
S3(config-if)# switchport mode access
S3(config-if)# switchport access vlan 20
S3(config-if)# switchport voice vlan 150
S3(config-if)# end
```

> [!NOTE]
> switchport access vlan 20 → Data VLAN 
> switchport voice vlan 150 → Voice VLAN

---
## Change VLAN Membership

To move a port from VLAN 20 to VLAN 30:

```text
S1(config)# interface fastEthernet 0/18
S1(config-if)# switchport access vlan 30
S1(config-if)# exit
```

The new VLAN assignment **replaces the previous one**.

To return the access VLAN setting to its default:

```text
S1(config)# interface fastEthernet 0/18
S1(config-if)# no switchport access vlan
S1(config-if)# exit
```

The port returns to the default access VLAN, **VLAN 1**.

---
## Delete a VLAN

Delete one VLAN:

```
S1# configure terminal
S1(config)# no vlan 20
S1(config)# end
```

> [!WARNING] Deleting a VLAN
> Deleting a VLAN **does not automatically move its access ports back to VLAN 1**.  
Ports assigned to the deleted VLAN become **inactive** and cannot forward traffic until the VLAN is recreated or the ports are assigned to an existing VLAN.

### Delete the VLAN Database

On switches that store VLAN information in `vlan.dat`:

```
S1# show flash
S1# delete flash:vlan.dat
S1# reload
```

This removes the VLAN database rather than simply one VLAN.

___
## Verification

```text
S1# show vlan brief
→ Displays VLAN IDs, names, status, and access ports

S1# show vlan id 20
→ Displays information about VLAN 20

S1# show vlan name STUDENT
→ Displays information about the specified VLAN

S1# show vlan summary
→ Displays a summary of the VLAN database

S1# show interfaces fastEthernet 0/18 switchport
→ Displays the Layer 2 configuration and VLAN membership of Fa0/18
```

---
## Gotchas

- VLANs are **separate Layer 2 broadcast domains**.
- VLANs must exist on the switches that need to use them.
- An access port normally carries traffic for **one data VLAN**.
- VLAN 1 is the **default VLAN** on Cisco switches.
- Assigning `switchport access vlan 30` replaces the previous access VLAN assignment.
- Deleting a VLAN **does not automatically reassign its access ports to VLAN 1**. Ports assigned to the deleted VLAN become inactive for that VLAN until the VLAN is recreated or the ports are reassigned.
- `show vlan brief` primarily shows **access-port VLAN membership**; trunk ports are better verified with trunk-specific commands.
- VLAN configuration is commonly stored separately from the startup configuration in the **`vlan.dat`** file on traditional Catalyst switches.
- Devices in different VLANs require **inter-VLAN routing** to communicate.

---
## Related

- [[MOC_Networking|Networking]]
- [[Trunking]]
