---
author: Alfeze
created: 2026-08-29
---
## DTP Configuration

> **DTP (Dynamic Trunking Protocol)** is a Cisco proprietary protocol used to dynamically negotiate whether a link operates as an **access link or trunk link**.

### Dynamic Auto

Passive mode. The interface becomes a trunk only if the neighbor actively attempts to form a trunk.

```
S1# configure terminal
S1(config)# interface gigabitEthernet 0/1
S1(config-if)# switchport mode dynamic auto
S1(config-if)# end
```

### Dynamic Desirable

Active mode. The interface actively attempts to negotiate a trunk with the neighbor.

```
S1# configure terminal
S1(config)# interface gigabitEthernet 0/1
S1(config-if)# switchport mode dynamic desirable
S1(config-if)# end
```

### Disable DTP

For a statically configured trunk, DTP negotiation can be disabled:

```
S1# configure terminal
S1(config)# interface gigabitEthernet 0/1
S1(config-if)# switchport mode trunk
S1(config-if)# switchport nonegotiate
S1(config-if)# end
```

For an end-device/access port, explicitly configure it as access:

```
S1# configure terminal
S1(config)# interface fastEthernet 0/18
S1(config-if)# switchport mode access
S1(config-if)# end
```

---
## DTP Negotiation

| S1                  | S2                  | Result     |
| ------------------- | ------------------- | ---------- |
| `access`            | `access`            | Access     |
| `access`            | `dynamic auto`      | Access     |
| `access`            | `dynamic desirable` | Access     |
| `dynamic auto`      | `dynamic auto`      | **Access** |
| `dynamic auto`      | `dynamic desirable` | **Trunk**  |
| `dynamic desirable` | `dynamic desirable` | **Trunk**  |
| `trunk`             | `dynamic auto`      | **Trunk**  |
| `trunk`             | `dynamic desirable` | **Trunk**  |
| `trunk`             | `trunk`             | **Trunk**  |

> [!important]  
> **Auto + Auto does NOT form a trunk.**  
> At least one side must actively attempt trunking (`dynamic desirable` or static `trunk`).

---
## Verification

```
S1# show dtp interface gigabitEthernet 0/1
→ Displays DTP information and the current negotiation state

S1# show interfaces gigabitEthernet 0/1 switchport
→ Displays administrative and operational switchport modes

S1# show interfaces trunk
→ Verifies whether the interface is actually operating as a trunk
```

---
## Gotchas

- DTP is **Cisco proprietary**.
- `dynamic auto` is **passive**.
- `dynamic desirable` is **active**.
- `switchport nonegotiate` disables DTP negotiation.
- Best practice is generally to **statically configure access/trunk ports instead of relying on DTP**.
- DTP availability and default behavior **depend on the Cisco platform and IOS version**.
- `switchport nonegotiate` does **not create a trunk**; configure `switchport mode trunk` separately.

---
## Related

- [[MOC_Networking|Networking]]

