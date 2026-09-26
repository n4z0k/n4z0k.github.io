---
author: Alfeze
created: 2026-08-30
---
# Spanning Tree Protocol (STP / Rapid-PVST+)

>Layer 2 network protocol designed to prevent loops (broadcast storms, multiple frame transmission, MAC table instability) by dynamically blocking redundant paths while keeping redundancy available. Cisco switches run Per-VLAN Spanning Tree (PVST+ / Rapid-PVST+) by default.

## Basic Configuration

### Set the STP mode (Rapid PVST+ recommended)

```text
Switch(config)# spanning-tree mode rapid-pvst
```

### Set root bridge priority for a VLAN (0 to 61440, multiple of 4096)

```text
Switch(config)# spanning-tree vlan 10 priority 4096
```

> [!NOTE]
> When configured manually, priority must be a multiple of 4096, otherwise rejected

### Force the switch to become primary root bridge (simpler alternative to priority)

```text
Switch(config)# spanning-tree vlan 10 root primary
```

> [!NOTE]
> "root primary" directly sets the priority to 24576 (2 * 4096 - base priority of 32768)

### Force the switch to become secondary root bridge (backup)

```text
Switch(config)# spanning-tree vlan 10 root secondary
```

## Verification

```text
! Full overview (root bridge, roles, states, priorities)
Switch# show spanning-tree

! Details for a specific VLAN
Switch# show spanning-tree vlan 10

! STP state of a specific interface
Switch# show spanning-tree interface gi1/0/1

! Identify the root bridge
Switch# show spanning-tree root

! Summary of active STP mode per VLAN
Switch# show spanning-tree summary
```

---
## Portfast and BPDU Guard

> A pair of access-port features that speed up connectivity by skipping STP's listening/learning states, while protecting against loops if an unauthorized switch is plugged in.

### Enable PortFast on an access port

```text
Switch(config-if)# spanning-tree portfast
```

### Enable PortFast by default on all access ports

```text
Switch(config)# spanning-tree portfast default
```

### Enable BPDU Guard on a port

 ```text
 Switch(config-if)# spanning-tree bpduguard enable
 ```

### Enable BPDU Guard globally with PortFast

```text
Switch(config)# spanning-tree portfast bpduguard default
```

### Enable Root Guard on a port (facing an untrusted switch)

```text
Switch(config-if)# spanning-tree guard root
```

### Enable Loop Guard on a port

```text
Switch(config-if)# spanning-tree guard loop
```

### Enable BPDU Filter on a port

```text
Switch(config-if)# spanning-tree bpdufilter enable
```

> [!NOTE]
> BPDU Filter prevents the port from sending/receiving BPDUs at all — different from BPDU Guard, which shuts the port down when a BPDU arrives.

### Example

```text
! Core switch (SW-CORE) designated as root bridge for VLAN 10 and 20
SW-CORE(config)# spanning-tree mode rapid-pvst
SW-CORE(config)# spanning-tree vlan 10,20 root primary

! Distribution switch as backup root
SW-DIST(config)# spanning-tree vlan 10,20 root secondary

! Access switch (SW-ACCESS) - port facing a PC
SW-ACCESS(config)# interface gi1/0/5
SW-ACCESS(config-if)# switchport mode access
SW-ACCESS(config-if)# spanning-tree portfast
SW-ACCESS(config-if)# spanning-tree bpduguard enable
```

---
## Troubleshooting

```text
! Check if a port is blocked in discarding/blocking state and why
Switch# show spanning-tree vlan 10 detail

! Check recent topology changes (TCN)
Switch# show spanning-tree vlan 10 | include Topology

! Check if BPDU Guard disabled a port (err-disabled)
Switch# show interfaces status err-disabled

! Re-enable an err-disabled port after a BPDU Guard event
Switch(config-if)# shutdown
Switch(config-if)# no shutdown

! Check BPDUs sent/received for debugging (use with caution in production)
Switch# debug spanning-tree events
```

## Gotchas

- PortFast must NEVER be enabled on a port connected to another switch: it skips listening/learning and goes straight to forwarding, creating a loop risk.
- The root bridge is elected based on the lowest Bridge ID (priority + MAC address) — default priority is 32768.
- Priority must be a multiple of 4096, otherwise the command is rejected.
- BPDU Guard puts the port into err-disabled state as soon as a BPDU is received — requires a manual `shutdown`/`no shutdown` or errdisable recovery to restore it.
- Root Guard prevents a port from becoming a root port if a superior BPDU is received (protects against an unwanted root bridge), unlike BPDU Guard which fully disables the port.
- Rapid PVST+ creates a separate STP instance per VLAN, which uses more CPU resources than MST on large topologies with many VLANs.
- Rapid PVST+ port states are: Discarding, Learning, Forwarding (unlike classic STP, which has Blocking, Listening, Learning, Forwarding).

Example 1

```text
! Root bridge election: compare Bridge IDs of two switches
SW1: Priority 32768 + MAC 0011.2233.4455 = lowest Bridge ID → SW1 becomes root
SW2: Priority 32768 + MAC 0022.3344.5566
```

Example 2

```text
! BPDU Guard scenario: a user plugs an unauthorized switch
! into an access port configured with portfast + bpduguard
! -> the port automatically goes into err-disabled state
! -> requires admin intervention or errdisable recovery
```

## Summary 

| Feature     | Triggers when...                                     | Where to use it                            |
| ----------- | ---------------------------------------------------- | ------------------------------------------ |
| PortFast    | Always active (no trigger, skips listening/learning) | Access ports (end devices)                 |
| BPDU Guard  | A BPDU **arrives** on a PortFast-enabled port        | Access ports (end devices)                 |
| BPDU Filter | Always (blocks BPDU send/receive)                    | Access ports, with caution                 |
| Root Guard  | A **superior** BPDU (better priority) is received    | Ports facing untrusted downstream switches |
| Loop Guard  | BPDUs **stop** arriving                              | Shared/trunk ports between switches        |

---
## Related

- [[MOC_Networking|Networking]]

