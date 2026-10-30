---
author: Alfeze
created: 2026-10-28
---

# Firewall Fundamentals

> A firewall is designed to inspect a network's or digital device’s incoming and outgoing traffic.

---
## Types of Firewalls 

![[Pasted image 20261028221723.png|361]]


| Firewalls                 | Characteristics                                                                                                                                                                                |
| ------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Stateless firewalls       | - Basic filtering  <br>- No track of previous connections  <br>- Efficient for high-speed networks                                                                                             |
| Stateful firewalls        | - Recognize traffic by patterns  <br>- Complex rules can be applicable  <br>- Monitor the network connections                                                                                  |
| Proxy firewalls           | - Inspect the data inside the packets as well  <br>- Provides content filtering options  <br>- Provides application control  <br>- Decrypts and inspects SSL/TLS data packets                  |
| Next-generation firewalls | - Provides advanced threat protection  <br>- Comes with an intrusion prevention system  <br>- Identify anomalies based on heuristic analysis  <br>- Decrypts and inspects SSL/TLS data packets |

---
## Linux iptables Firewall

### Netfilter 

Netfilter is the framework inside the Linux OS with core firewall functionalities, including packet filtering, NAT, and connection tracking. This framework serves as the foundation for various firewall utilities available in Linux to control network traffic. Some common firewall utilities that utilize this framework are listed below:

- `iptables`: This is the most widely used utility in many Linux distributions. It uses the Netfilter framework that provides various functionalities to control network traffic.
- `nftables`: It is a successor to the “iptables” utility, with enhanced packet filtering and NAT capabilities. It is also based on the Netfilter framework.
- `firewalld`: This utility also operates on the Netfilter framework and has predefined rule sets. It works differently from the others and comes with different pre-built network zone configurations.

### ufw 

ufw (Uncomplicated Firewall), as the name says, eliminates the complications of making rules in a complex syntax in “iptables”(or its successor) by giving you an easier interface. It is more beginner-friendly. Basically, whatever rules you need in “iptables”, you can define them with some easy commands via ufw, which would then be configuring your desired rules in “iptables”. Let’s have a look at some basic ufw commands down below.

To check the status of the firewall, you could use the command below:

```shell
sudo ufw status
```

If it appears inactive, you can enable it using the following command:

```shell
sudo ufw enable
```

To turn off the firewall, type “disable” instead of “enable” in the above command.

Below is a rule created to allow all the outgoing connections from a Linux machine. The `default` in the command means that we are defining this policy as a default policy allowing all the outgoing traffic unless we define an outgoing traffic restriction on any specific application in a separate rule. 

You can also make a rule to allow/deny traffic coming into your machine by replacing `outgoing` with `incoming` in the following command:

(Allow all outgoing traffic)

```shell
sudo ufw default allow outgoing
```

You can deny incoming traffic at any port in your system. Let's say that we want to block incoming SSH traffic. We can achieve this with the command `ufw deny 22/tcp`. As you can see, we first specified the action, `deny` in this case; furthermore, we specified the port and the transport protocol, which is TCP port 22, or simply `22/tcp`.

(Deny incoming SSH traffic)

```shell
sudo ufw deny 22/tcp
```

To list down all the active rules in a numbered order, you can use the following command:

```shell
sudo ufw status numbered
```

To delete any rule, execute the following command with the rule number to delete:

```shell
sudo ufw delete 2
```

---

## Related

- [[MOC_Security|Security]]

