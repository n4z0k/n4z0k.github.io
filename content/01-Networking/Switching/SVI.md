---
author: Alfeze
created: 2026-08-29
---
# SVI_Configuration

> A Switch Virtual Interface (SVI) is a logical Layer 3 interface associated with a VLAN. On a Layer 2 switch, an SVI is commonly used to assign an IP address to the switch for remote management.

---
## Configuration

### SVI + Management VLAN + Access Port

It is recommended to use a **dedicated management VLAN** instead of the default VLAN 1.

Example using **VLAN 99**:

```text
S1# configure terminal

S1(config)# vlan 99
S1(config-vlan)# name MANAGEMENT
S1(config-vlan)# exit

S1(config)# interface gigabitEthernet 0/1
S1(config-if)# switchport mode access
S1(config-if)# switchport access vlan 99
S1(config-if)# no shutdown
S1(config-if)# exit

S1(config)# interface vlan 99
S1(config-if)# ip address 172.17.99.11 255.255.255.0
S1(config-if)# ipv6 address 2001:DB8:ACAD:99::1/64
S1(config-if)# no shutdown
S1(config-if)# exit

S1(config)# ip default-gateway 172.17.99.1

S1(config)# end
S1# copy running-config startup-config
```

---
## Verification / Troubleshooting

```text
S1# show vlan brief
S1# show interfaces vlan 99
S1# show ip interface brief
S1# show ipv6 interface brief
```

Test connectivity to the default gateway:

```text
S1# ping 172.17.99.1
```

---
## Gotchas

- An **SVI is a logical interface**, not a physical switch port.
- An SVI is created/configured with `interface vlan <vlan-id>`.
- On a Layer 2 switch, an SVI is commonly used as the **management interface**.
- VLAN 1 is the default VLAN, but using a **dedicated management VLAN** such as VLAN 99 is recommended.
- The management VLAN must exist before its SVI can become operational.
- `no shutdown` administratively enables the SVI.
- `no shutdown` alone does **not** guarantee that the SVI will become `up/up`.
- For the SVI to become operational, the associated VLAN must be active and normally have at least one active Layer 2 port in that VLAN, either directly as an access port or through an active trunk carrying that VLAN.
- `ip default-gateway` allows a Layer 2 switch to communicate with devices located outside its local IPv4 subnet.
- The default gateway must normally be the IP address of a router or Layer 3 interface reachable through the management VLAN.
- A Layer 2 switch does not require an IPv4 default gateway to communicate with devices in the **same subnet**.
- For IPv6 management, the switch can learn its IPv6 default router through **Router Advertisement (RA)** messages.
- Some older Catalyst platforms/IOS versions may require an appropriate SDM template and a reload before IPv6 features are available.
- Always save the configuration with `copy running-config startup-config` if it must survive a reboot.

---
## Related

- [[MOC_Networking|Networking]]

