---
author: Alfeze
created: 2026-08-29
---

# Trunking

> A trunk link carries traffic for multiple VLANs between network devices, typically between switches or between a switch and a router.

---
##  Trunk Configuration

```text
S1# configure terminal

S1(config)# interface gigabitEthernet 0/1
S1(config-if)# switchport mode trunk
S1(config-if)# switchport trunk allowed vlan 10,20,30,99
S1(config-if)# switchport trunk native vlan 99
S1(config-if)# switchport nonegotiate
S1(config-if)# no shutdown
S1(config-if)# exit
```

```text
switchport mode trunk
→ Forces the interface to operate as a trunk port

switchport trunk native vlan 99
→ Configures VLAN 99 as the native VLAN

switchport trunk allowed vlan 10,20,30,99
→ Allows only VLANs 10, 20, 30, and 99 on the trunk

S1(config-if)# switchport nonegotiate 
→ Disables DTP negotiation on the interface.
```

---
## Native VLAN

> The native VLAN carries **untagged traffic** on an 802.1Q trunk.

```
S1(config-if)# switchport trunk native vlan 99
```

By default, the native VLAN is usually:

```
VLAN 1
```

For security, it is common to change the native VLAN to an unused VLAN.

> The native VLAN must match on both ends of the trunk.

Example:

```
S1(config)# interface gigabitEthernet 0/1
S1(config-if)# switchport trunk native vlan 99
```

```
S2(config)# interface gigabitEthernet 0/1
S2(config-if)# switchport trunk native vlan 99
```

---
## Gotchas

- A trunk carries traffic for **multiple VLANs**.
- `switchport mode trunk` manually configures the interface as a trunk.
- IEEE **802.1Q** is the trunking standard used on modern Cisco Ethernet networks.
- Frames are normally tagged with a VLAN ID on a trunk.
- Frames belonging to the **native VLAN are normally sent untagged**.
- The default native VLAN is usually **VLAN 1**.
- The native VLAN should match on **both ends of the trunk**.
- A native VLAN mismatch can cause connectivity issues and CDP warnings.
- `switchport trunk allowed vlan ...` controls which VLANs may cross the trunk.
- VLANs must exist on the switch if they are going to be used.
- `show vlan brief` does not fully show trunk behavior; use `show interfaces trunk`.
- On some older Cisco platforms, trunk encapsulation may need to be selected before trunk mode, for example `switchport trunk encapsulation dot1q`, but many modern Catalyst switches support only 802.1Q and do not use that command.
- For security, avoid using VLAN 1 as the native or management VLAN when possible.

---
## Related

- [[MOC_Networking|Networking]]
- [[DTP]]



