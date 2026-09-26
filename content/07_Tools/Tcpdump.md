---
author: Alfeze
created: 2026-09-20
---

# Tcpdump

> **tcpdump** is a powerful command-line packet analyzer built on the `libpcap` library. It captures, filters, and analyzes network traffic passing through a specific interface in real time, with the ability to export captures to `.pcap` files for deep troubleshooting, security audits, and protocol analysis.

---
## Basic Packet Capture

> [!NOTE]
>  It is important to note that capturing packets requires you to be logged-in as `root` or to use `sudo`


- List Available Network Interfaces for Capture

>`tcpdump -D`

- Specify the Network Interface 

> `-i INTERFACE`

- Save the Captured Packets 

>`-w FILE.pcap`     

- Read Captured Packets from a File 

>`-r FILE`

- Limit the Number of Captured Packets

>`-c COUNT`

- Don't Resolve IP Address

> `-n`  

- Don't Resolve Port Numbers

>`-nn`

- Produce (More) Verbose Output

>`-v`   or `-vv`  or `-vvv`

### Summary and Examples

| Command                | Explanation                                                         |
| ---------------------- | ------------------------------------------------------------------- |
| `tcpdump -i INTERFACE` | Captures packets on a specific network interface                    |
| `tcpdump -w FILE`      | Writes captured packets to a file                                   |
| `tcpdump -r FILE`      | Reads captured packets from a file                                  |
| `tcpdump -c COUNT`     | Captures a specific number of packets                               |
| `tcpdump -n`           | Don’t resolve IP addresses                                          |
| `tcpdump -nn`          | Don’t resolve IP addresses and don’t resolve protocol numbers       |
| `tcpdump -v`           | Verbose display; verbosity can be increased with `-vv`   and `-vvv` |

Consider the following examples:

- `tcpdump -i eth0 -c 50 -v` captures and displays 50 packets by listening on the `eth0` interface, which is a wired Ethernet, and displays them verbosely.

- `tcpdump -i wlo1 -w data.pcap` captures packets by listening on the `wlo1` interface (the WiFi interface) and writes the packets to `data.pcap`. It will continue till the user interrupts the capture by pressing CTRL-C.

- `tcpdump -i any -nn` captures packets on all interfaces and displays them on screen without domain name or protocol resolution.

---
## Filtering Expression

### Filtering by Host

> `host IP` or `host HOSTNAME`
> 

Example :

```bash
sudo tcpdump host example.com -w http.pcap
```

If you want to limit the packets to those from a particular source IP address or hostname, you must use `src host IP` or `src host HOSTNAME`. Similarly, you can limit packets to those sent to a specific destination using `dst host IP` or `dst host HOSTNAME`.

### Filtering by Port

> `port 53`

Example : 

```bash
sudo tcpdump -i ens5 port 53 -n
```

In the above example, we captured all the packets sent to or from a specific port number. You can limit the packets to those from a particular source port number or to a particular destination port number using `src port PORT_NUMBER` and `dst port PORT_NUMBER`, respectively.

### Filtering by Protocol

>You can limit your packet capture to a specific protocol; examples include: `ip`, `ip6`, `udp`, `tcp`, and `icmp`.


Example :

```bash
sudo tcpdump -i ens5 icmp -n
```

In the example above, we limit our packet capture to ICMP packets

### Logical Operators

Three logical operators that can be handy:

- `and`: Captures packets where both conditions are true. For example, `tcpdump host 1.1.1.1 and tcp` captures `tcp` traffic with `host 1.1.1.1`.
- `or`: Captures packets when either one of the conditions is true. For instance, `tcpdump udp or icmp` captures UDP or ICMP traffic.
- `not`: Captures packets when the condition is not true. For example, `tcpdump not tcp` captures all packets except TCP segments; we expect to find UDP, ICMP, and ARP packets among the results.

### Summary and Examples

| Command                                      | Explanation                                                           |
| -------------------------------------------- | --------------------------------------------------------------------- |
| `tcpdump host IP` or `tcpdump host HOSTNAME` | Filters packets by IP address or hostname                             |
| `tcpdump src host IP`                        | Filters packets by a specific source host                             |
| `tcpdump dst host IP`                        | Filters packets by a specific destination host                        |
| `tcpdump port PORT_NUMBER`                   | Filters packets by port number                                        |
| `Filters packets by port number`             | Filters packets by the specified source port number                   |
| `tcpdump dst port PORT_NUMBER`               | Filters packets by the specified destination port number              |
| `tcpdump PROTOCOL`                           | Filters packets by protocol; examples include `ip`, `ip6`, and `icmp` |
Consider the following examples:

- `tcpdump -i any tcp port 22` listens on all interfaces and captures `tcp` packets to or from `port 22`, i.e., SSH traffic.
- `tcpdump -i wlo1 udp port 123` listens on the WiFi network card and filters `udp` traffic to `port 123`, the Network Time Protocol (NTP).
- `tcpdump -i eth0 host example.com and tcp port 443 -w https.pcap` will listen on `eth0`, the wired Ethernet interface and filter traffic exchanged with `example.com` that uses `tcp` and `port 443`. In other words, this command is filtering HTTPS traffic related to `example.com`.

---
## Advanced Filtering


>we can limit the displayed packets to those smaller or larger than a certain length:

- `greater LENGTH`: Filters packets that have a length greater than or equal to the specified length
- `less LENGTH`: Filters packets that have a length less than or equal to the specified length

> [!TIP]
> We recommend you check the `pcap-filter` manual page by issuing the command `man pcap-filter`

---
## Displaying Packets

>Tcpdump is a rich program with many options to customize how the packets are printed and displayed. We have selected to cover the following five options:

- `-q`: Quick output; print brief packet information
- `-e`: Print the link-level header
- `-A`: Show packet data in ASCII
- `-xx`: Show packet data in hexadecimal format, referred to as hex
- `-X`: Show packet headers and data in hex and ASCII

| Command       | Explanation                                        |
| ------------- | -------------------------------------------------- |
| `tcpdump -q`  | Quick and quite: brief packet information          |
| `tcpdump -e`  | Include MAC addresses                              |
| `tcpdump -A`  | Print packets as ASCII encoding                    |
| `tcpdump -xx` | Display packets in hexadecimal format              |
| `tcpdump -X`  | Show packets in both hexadecimal and ASCII formats |

---
## Related 

- [[MOC_Tools|Tools]]
