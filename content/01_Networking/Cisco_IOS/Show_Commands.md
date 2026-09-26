---
author: Alfeze
created: 2026-08-29
---

# Show Commands

> Essential Cisco IOS verification and troubleshooting commands.

---
## Verification Commands

```
show version
show running-config 
show startup-config
show history

show ip interface brief
show ipv6 interface brief

show ip route
show ipv6 route

show interfaces
show ip interface g0/0/0
show ipv6 interface g0/0/0

show ip interfaces
show ipv6 interfaces

show ip arp 
show arp 

show protocols 

show version 

show cdp neighbors 
show cdp neighbors detail 

show ip ports all

show flash
show mac address-table

```

---
## Debug Commands

>`debug` commands display protocol events and system activity in real time. They are used for troubleshooting and should be disabled when no longer needed.

```text
R1# debug ?
→ Displays all available debugging options

R1# debug ip icmp
→ Enables real-time debugging of ICMP messages

R1# undebug ip icmp
→ Disables ICMP debugging

R1# undebug all
→ Disables all active debugging
```

> [!WARNING]
>`debug` can consume significant CPU resources and generate a large amount of output. Always disable debugging when finished.

---
##  Terminal monitor Command

> By default, debug and logging messages may not appear in a remote VTY session such as SSH. `terminal monitor` enables these messages for the current remote session.

```text
R1# terminal monitor
→ Displays debug and logging messages in the current VTY (SSH/Telnet) session

R1# terminal no monitor
→ Stops displaying debug and logging messages in the current VTY session
```

> [!TIP]
> `terminal monitor` is mainly useful when connected remotely through SSH or Telnet. It is normally not required when connected directly through the console port.

---
## Useful Filters

```
R1# show running-config | include <text>
→ Display only lines containing specific text

R1# show running-config | section <text>
→ Display an entire configuration section

R1# show running-config | begin <text>
→ Start displaying output from a specific line

R1# show running-config | exclude <text>
→ Exclude lines containing specific text
```

---
## Related

- [[MOC_Networking|Networking]]
