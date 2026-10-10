---
author: Alfeze
created: 2026-09-20
---

# Nmap

>Nmap is an open-source network scanner that was first published in 1997. Since then, plenty of features and options have been added. It is a powerful and flexible network scanner that can be adapted to various scenarios and setups.

[Nmap.org](https://nmap.org/)

---
## Specifying Targets

Nmap accepts several target formats:

- **IP range using `-`**: If you want to scan all the IP addresses from 192.168.0.1 to 192.168.0.10, you can write `192.168.0.1-10`
- **IP subnet using `/`**: If you want to scan a subnet, you can express it as `192.168.0.1/24`, and this would be equivalent to `192.168.0.0-255`
- **Hostname**: You can also specify your target by hostname, for example, `example.thm`

--- 
## Host Discovery `-sn` vs `-sL`

- Ping Sweep (No Port Scan) Checks which hosts are online without scanning open ports. 

```bash
nmap -sn 192.168.66.0/24
```

>**Packets sent directly to targets:** 
>**Local network (Ethernet)**: **ARP** requests (if host replies $\rightarrow$ `Host is up`). 
>**Remote network (routed / non-local)**: **ICMP Echo**, TCP SYN (port 443), TCP ACK (port 80), and ICMP Timestamp. 
>**Port scan**: none. 


- List Scan : lists targets and resolves their hostnames without sending anny traffic to them.

```bash
nmap -sL 192.168.0.1/24
```

>**Zero packets sent to targets** : completely passive toward the target hosts.
>**Reverse DNS resolution (rDNS / PTR)**:  queries only the configured DNS server to resolve names for each IP.
>**Port scan / ping**: none.

**Key takeaway:**

- `-sn` = active host discovery (ARP / ICMP / TCP sent directly to hosts).
- `-sL` = passive enumeration (DNS queries only, zero packets to targets).

---
## Port Scanning

>Connect Scan

`-sT` : It tries to complete the TCP three-way handshake with every target TCP port. If the TCP port turns out to be open and Nmap connects successfully, Nmap will tear down the established connection.

>SYN Scan (Stealth)

`sS` : he SYN scan only executes the first step: it sends a TCP SYN packet. Consequently, the TCP three-way handshake is never completed. The advantage is that this is expected to lead to fewer logs as the connection is never established, and hence, it is considered a relatively stealthy scan. You can select the SYN scan using the -sS flag.


>Scanning UDP Ports

`-sU`

---
## Limiting the Target Ports

Nmap scans the most common 1,000 ports by default. However, this might not be what you are looking for. Therefore, Nmap offers you a few more options.

- `-F` is for Fast mode, which scans the 100 most common ports (instead of the default 1000).

- `-p[range]` allows you to specify a range of ports to scan. For example, `-p10-1024` scans from port 10 to port 1024, while `-p-25` will scan all the ports between 1 and 25. Note that `-p-` scans all the ports and is equivalent to `-p1-65535` and is the best option if you want to be as thorough as possible.

- Tip: The most common services use a port number between 1 and 1024 for either UDP or TCP. These ports are also known as **well-known ports**. Use `-p1-1023` to scan for the well-known ports.

| Option      | Explanation                                                 |
| ----------- | ----------------------------------------------------------- |
| `-sT`       | TCP connect scan – complete three-way handshake             |
| `-sS`       | TCP SYN – only first step of the three-way handshake        |
| `-sU`       | UDP scan                                                    |
| `-F`        | Fast mode – scans the 100 most common ports                 |
| `-p[range]` | Specifies a range of port numbers `-p-` scans all the ports |

---

 **OS Detection**

`-O` enable OS detection

**Service and Version Detection**

`-sV` enables version detection

`-A` enables OS detection, version scanning, and traceroute, among other things

**Forcing the Scan** 

`-Pn`  We can ask Nmap to treat all hosts as online and port scan every host, including those that didn’t respond during the host discovery phase.

Summary

| Option | Explanation                                          |
| ------ | ---------------------------------------------------- |
| `-O`   | OS detection                                         |
| `-sV`  | Service and version detection                        |
| `-A`   | OS detection, version detection, and other additions |
| `-Pn`  | Scan hosts that appear to be down                    |

---
## Timing 

>Nmap gives you six timing templates, and the names say it all: paranoid (0), sneaky (1), polite (2), normal (3), aggressive (4), and insane (5). 

| Option                                                              | Explanation                                                                                        |
| ------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| `-T<0-5>`                                                           | Timing template – paranoid (0), sneaky (1), polite (2), normal (3), aggressive (4), and insane (5) |
| `--min-parallelism <numprobes>` and `--max-parallelism <numprobes>` | Minimum and maximum number of parallel probes                                                      |
| `--min-rate <number>` and `--max-rate <number>`                     | Minimum and maximum rate (packets/second)                                                          |
| `--host-timeout`                                                    | Maximum amount of time to wait for a target host                                                   |

---
## Output 

`-v` enable verbose output (verbosity)

>you can increase the verbosity level by adding another “v” such as -vv or even -vvvv. You can also specify the verbosity level directly, for example, -v2 and -v4. You can even increase the verbosity level by pressing “v” after the scan already started.

If all this verbosity does not satisfy your needs, you must consider the -d for debugging-level output. Similarly, you can increase the debugging level by adding one or more “d” or by specifying the debugging level directly. The maximum level is -d9; before choosing that, make sure you are ready for thousands of information and debugging lines.

Saving Scan Report

- `-oN <filename>` - Normal output
- `-oX <filename>` - XML output
- `-oG <filename>` -  grep-able output (useful for `grep` and `awk`)
- `-oA <basename>` -  Output in all major formats

---
## Summary

| Option                                                              | Explanation                                                                                        |
| ------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| `-sL`                                                               | List scan – list targets without scanning                                                          |
| *Host Discovery*                                                    |                                                                                                    |
| `-sn`                                                               | Ping scan – host discovery only                                                                    |
| *Port Scanning*                                                     |                                                                                                    |
| `-sT`                                                               | TCP connect scan – complete three-way handshake                                                    |
| `-sS`                                                               | TCP SYN – only first step of the three-way handshake                                               |
| `-sU`                                                               | UDP Scan                                                                                           |
| `-F`                                                                | Fast mode – scans the 100 most common ports                                                        |
| `-p[range]`                                                         | Specifies a range of port numbers  `-p-` scans all the ports                                       |
| `-Pn`                                                               | Treat all hosts as online – scan hosts that appear to be down                                      |
| *Service Detection*                                                 |                                                                                                    |
| `-O`                                                                | OS detection                                                                                       |
| `-sV`                                                               | Service version detection                                                                          |
| `-A`                                                                | OS detection, version detection, and other additions                                               |
| *Timing*                                                            |                                                                                                    |
| `-T<0-5>`                                                           | Timing template – paranoid (0), sneaky (1), polite (2), normal (3), aggressive (4), and insane (5) |
| `--min-parallelism <numprobes>` and `--max-parallelism <numprobes>` | Minimum and maximum number of parallel probes                                                      |
| `--min-rate <number>` and `--max-rate <number>`                     | Minimum and maximum rate (packets/second)                                                          |
| `--host-timeout`                                                    | Maximum amount of time to wait for a target host                                                   |
| *Real-time output*                                                  |                                                                                                    |
| `-v`                                                                | Verbosity level – for example, `-vv` and `-v4`                                                     |
| `-d`                                                                | Debugging level – for example `-d` and `-d9`                                                       |
| *Report*                                                            |                                                                                                    |
| `-oN <filename>`                                                    | Normal output                                                                                      |
| `-oX <filename>`                                                    | XML output                                                                                         |
| `-oG <filename>`                                                    | `grep`-able output                                                                                 |
| `-oA <basename>`                                                    | Output in all major formats                                                                        |

---
## Usage Examples

```shell
nmap -T4 -vv -F -Pn -sV --script vulners -oN scan.txt <IP_Cible>
```


---

## Related 

- [[MOC_Tools|Tools]]
