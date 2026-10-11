---
author: Alfeze
created: 2026-10-10
---

# Gobuster

> Gobuster is an open-source offensive tool written in Golang. It enumerates web directories, DNS subdomains, vhosts, Amazon S3 buckets, and Google Cloud Storage by brute force, using specific wordlists and handling the incoming responses. Many security professionals use this tool for penetration testing, bug bounty hunting, and cyber security assessments. Looking at the phases of ethical hacking, we can place Gobuster between the reconnaissance and scanning phases.

---
## Introduction 

**Enumeration**

Enumeration is the act of listing all the available resources, whether they are accessible or not. For example, Gobuster enumerates web directories.

**Brute Force**

Brute force is the act of trying every possibility until a match is found. It is like having ten keys and trying them all on a lock until one fits. Gobuster uses wordlists for this purpose.

Several tools exist to brute-force web directories and files. They differ mainly in language, interface, and speed:

| Tool            | Language | Interface | Speed                               |
| --------------- | -------- | --------- | ----------------------------------- |
| **Feroxbuster** | Rust     | CLI       | ⚡⚡ Very fast (recursive by default) |
| **Gobuster**    | Go       | CLI       | ⚡ Fast                              |
| **DirBuster**   | Java     | GUI       | 🐢 Slow                             |
| **Dirb**        | C        | CLI       | 🐌 Single-threaded                  |

---
## Overview

```shell
gobuster --help
```

 You should get the help page for the Gobuster tool.

![[Pasted image 20261010181750.png]]

The help page contains multiple sections:

- **Usage**: Shows the syntax on how to use the command.

- **Available Commands**: Multiple commands are available to aid us in enumerating directories, files, DNS subdomains, Google Cloud Storage buckets, and Amazon AWS S3 buckets. Throughout this room, we will focus on the dir, dns, and vhost commands. 

- **Flags**: These are specific options we can configure to customize our commands.

| Short Flag | Long Flag    | Description                                                                                                                                                                                                                                                                                                            |
| ---------- | ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-t`       | `--threads`  | This flag configures the number of threads to use for the scan. Each of these threads sends out requests with a slight delay. The default number of threads is 10. This number may be slow when using large wordlists. You can increase or decrease the number of threads depending on the available system resources. |
| `-w`       | `--wordlist` | The flag configures a wordlist to use for iterating. Each wordlist entry is attached to the URL you included in the command.                                                                                                                                                                                           |
| ``         | `--delay`    | This flag defines the amount of time to wait between sending requests. Some web servers include mechanisms to detect enumeration by looking at how many requests are received in a certain period of time. We can increase the delay between subsequent requests to make it look like normal web traffic.              |
| ``         | `--debug`    | This flag helps us to troubleshoot when our command gives unexpected errors.                                                                                                                                                                                                                                           |
| `-o`       | `--output`   | This flag writes the enumeration results to a file we choose.                                                                                                                                                                                                                                                          |

**Example**

```shell
gobuster dir -u "http://www.example.thm/" -w /usr/share/wordlists/dirb/small.txt -t 64
```

- `gobuster dir` indicates that we will use the directory and file enumeration mode.
- -u "http://www.example.thm/" tells Gobuster that the target URL is http://example.thm/.
- `-w /usr/share/wordlists/dirb/small.txt` directs Gobuster to use the small.txt wordlist to brute force the web directories. Gobuster will use each entry in the wordlist to form a new URL and send a GET request to that URL. If the first entry of the wordlist were images, Gobuster would send a GET request to http://example.thm/images/.
- `-t 64` sets the number of threads Gobuster will use to 64. This improves the performance drastically.

---
## Directory and File Enumeration mode (dir)

```shell
gobuster dir --help
```

Many flags are used to fine-tune the `gobuster dir` command. 

| Flag | Long Flag                  | Description                                                                                                                                                                                                                   |
| ---- | -------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-c` | `--cookies`                | This flag configures a cookie to pass along each request, such as a session ID.                                                                                                                                               |
| `-x` | `--extensions`             | This flag specifies which file extensions you want to scan for. E.g., .php, .js                                                                                                                                               |
| `-H` | `--headers`                | This flag configures an entire header to pass along with each request.                                                                                                                                                        |
| `-k` | `--no-tls-validation`      | This flag  skips the process that checks the certificate when https is used. It often happens for CTF events or test rooms like the ones on THM a self-signed certificate is used. This causes an error during the TLS check. |
| `-n` | `--no-status`              | You can set this flag when you don’t want to see status codes of each response received. This helps keep the output on the screen clear.                                                                                      |
| `-P` | `password`                 | You can set this flag together with the --username flag to execute authenticated requests. This is handy when you have obtained credentials from a user.                                                                      |
| `-s` | `--status-codes`           | With this flag, you can configure which status codes of the received responses you want to display, such as 200, or a range like 300-400.                                                                                     |
| `-b` | `--status-codes-blacklist` | This flag allows you to configure which status codes of the received responses you don’t want to display. Configuring this flag overrides the -s flag.                                                                        |
| `-U` | `--username`               | You can set this flag together with the --password flag to execute authenticated requests. This is handy when you have obtained credentials from a user.                                                                      |
| `-r` | `--followredirect`         | This flags configures Gobuster to follow the redirect that it received as a response to the sent request. A HTTP redirect status code (e.g., 301 or 302) is used to redirect the client to a different URL.                   |

To run Gobuster in `dir` mode, use the following command format:

```shell
gobuster dir -u "http://www.example.thm" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -r
```

Notice that the command also includes the flags `-u` and `-w`, in addition to the dir keyword. These two flags are required for the Gobuster directory enumeration to work.

Let’s look at a second example where we use the `-x` flag to specify what type of files we want to enumerate:

```shell
gobuster dir -u "http://www.example.thm" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x .php,.js
```

---
## Subdomain Enumeration  mode (dns)

```shell
gobuster dns --help
```

The `dns` mode offers fewer flags than the `dir` mode. But these are more than enough to cover most DNS subdomain enumeration scenarios. Let us have a look at some of the commonly used flags:

| Flag | Long Flag      | Description                                                                       |
| ---- | -------------- | --------------------------------------------------------------------------------- |
| `-c` | `--show-cname` | Show CNAME Records (cannot be used with the -`i` flag).                           |
| `-i` | `--show-ips`   | Including this flag shows IP addresses that the domain and subdomains resolve to. |
| `-r` | `--resolver`   | This flag configures a custom DNS server to use for resolving.                    |
| `-d` | `--domain`     | This flag configures the domain you want to enumerate.                            |

To run Gobuster in `dns` mode, use the following command syntax:

```shell
gobuster dns -d example.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
```

Notice that the command also includes the flags `-d` and `-w`, in addition to the `dns` keyword. These two flags are required for the Gobuster subdomain enumeration to work.

---
## Vhost Enumeration mode (vhost)

The  difference between `vhost` and `dns` mode is in the way Gobuster scans:

- `vhost` mode will navigate to the URL created by combining the configured HOSTNAME (-u flag) with an entry of a wordlist.
- `dns` mode will do a DNS lookup to the FQDN created by combining the configured domain name (-d flag) with an entry of a wordlist.

```shell
gobuster vhost --help
```

The `vhost` mode offers flags similar to those of the dir mode. Let us have a look at some of the commonly used flags:

| Short Flag | Long Flag           | Description                                                                                           |
| ---------- | ------------------- | ----------------------------------------------------------------------------------------------------- |
| `-u`       | `--url`             | Specifies the base URL (target domain) for brute-forcing virtual hostnames.                           |
|            | `--append-domain`   | Appends the base domain to each word in the wordlist (e.g., word.example.com).                        |
| `-m`       | `--method`          | Specifies the HTTP method to use for the requests (e.g., GET, POST).                                  |
|            | `--domain`          | Appends a domain to each wordlist entry to form a valid hostname (useful if not provided explicitly). |
|            | `--exclude-length`  | Excludes results based on the length of the response body (useful to filter out unwanted responses).  |
| `-r`       | `--follow-redirect` | Follows HTTP redirects (useful for cases where subdomains may redirect).                              |

To run Gobuster in `vhost` mode, type the following command:

```shell
gobuster vhost -u "http://10.129.177.5" --domain example.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt --append-domain --exclude-length 250-320 
```

Notice that the command also includes the flags -u and -w, in addition to the vhost keyword. These two flags are required for the Gobuster vhost enumeration to work.

---
## Related 

- [[MOC_Tools|Tools]]
