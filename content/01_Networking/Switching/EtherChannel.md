---
author: Alfeze
created: 2026-09-02
---

# EtherChannel

> EtherChannel A Layer 2/Layer 3 technology that bundles multiple physical links between two switches into a single logical link, providing increased bandwidth, redundancy, and load balancing while STP sees it as one link (avoiding blocked redundant ports). Negotiated dynamically using LACP (IEEE 802.3ad, open standard) or PAgP (Cisco proprietary), or configured statically with `on` mode.

---
## Configuration

### LACP
#### Configure a port-channel using LACP (active mode — initiates negotiation)

```text
Switch(config)# interface range gi1/0/1-2
Switch(config-if-range)# channel-group 1 mode active
```

#### Configure a port-channel using LACP (passive mode — waits for negotiation)

```text
Switch(config)# interface range gi1/0/1-2 Switch(config-if-range)# channel-group 1 mode passive
```

> [!NOTE]
> NOTE: at least one side must be `active` — two `passive` sides will never form a channel.

---
### PAgP

#### Configure a port-channel using PAgP (desirable mode — initiates negotiation)

```text
Switch(config)# interface range gi1/0/1-2
Switch(config-if-range)# channel-group 1 mode desirable
```

#### Configure a port-channel using PAgP (auto mode — waits for negotiation)

```text
Switch(config)# interface range gi1/0/1-2
Switch(config-if-range)# channel-group 1 mode auto
```

---

### Static port-channel

#### Configure a static port-channel (no negotiation protocol)

```text
Switch(config)# interface range gi1/0/1-2
Switch(config-if-range)# channel-group 1 mode on
```

> [!NOTE]
> `on` mode forms the channel unconditionally with no negotiation — both sides must also be set to `on`, otherwise it can cause a loop since STP has no way to detect a misconfiguration.

---
## Configure the logical port-channel interface (trunk example)

```text
Switch(config)# interface port-channel 1
Switch(config-if)# switchport trunk encapsulation dot1q
Switch(config-if)# switchport mode trunk
```

---
## Verification

```
! Overview of all EtherChannel groups and their protocol (LACP/PAgP/static)
Switch# show etherchannel summary

! Detailed info for a specific port-channel group
Switch# show etherchannel port-channel

! LACP-specific neighbor and port state info
Switch# show lacp neighbor

! PAgP-specific neighbor info
Switch# show pagp neighbor

! Global status of the logical port-channel interface
Switch# show interfaces port-channel 1

! Role of each physical member interface within the EtherChannel
Switch# show interfaces etherchannel
```

## Troubleshooting

```text
! Check for mode mismatches (e.g. active/desirable mismatch prevents bundling)
Switch# show etherchannel summary
! Look for flags: SU = in use, P = bundled in port-channel, I = individual (not bundled), D = down

! Verify all bundled physical ports have identical config (speed, duplex, VLAN, trunk settings)
Switch# show running-config interface gi1/0/1
Switch# show running-config interface gi1/0/2

! Check for suspended ports due to config mismatch
Switch# show interfaces gi1/0/1 etherchannel
```

### Gotchas

- All bundled physical ports must have **matching configuration**: speed, duplex, trunk mode, allowed VLANs, native VLAN. A mismatch prevents the port from joining the channel (it stays in `suspended` or `individual` state).
- LACP mode compatibility: `active`-`active` ✅, `active`-`passive` ✅, `passive`-`passive` ❌ (never negotiates).
- PAgP mode compatibility: `desirable`-`desirable` ✅, `desirable`-`auto` ✅, `auto`-`auto` ❌ (never negotiates).
- `on` mode bypasses negotiation entirely — both ends must be manually set to `on`, or it risks creating a Layer 2 loop invisible to STP.
- LACP is an open IEEE standard (802.3ad) and works across vendors; PAgP is Cisco-proprietary and only works between Cisco devices.
- Maximum of 8 active links per EtherChannel bundle (with up to 8 more in standby with LACP, for a total of 16 physical interfaces per group).
- Load balancing is based on a hash (source/destination MAC, IP, or port depending on platform) — it does NOT guarantee equal traffic distribution across links, especially with few flows.
- STP treats an EtherChannel as a single logical link, so it does not block any of the bundled physical ports — this is one of the main reasons to use EtherChannel in redundant topologies.

---
## Related

- [[MOC_Networking|Networking]]

