---
author: Alfeze
created: 2026-10-10
---

# Hydra

> Hydra is a brute force online password cracking program, a quick system login password “hacking” tool.

---
## Supports protocols

Hydra supports and has the ability to brute force the following protocols:

```text
Asterisk, AFP, Cisco AAA, Cisco auth, Cisco enable, CVS, Firebird, FTP, HTTP-FORM GET, HTTP-FORM-POST, HTTP-GET, HTTP-HEAD, HTTP-POST, HTTP-PROXY, HTTPS-FORM-GET, HTTPS-FORM-POST, HTTPS-GET, HTTPS-HEAD, HTTPS-POST, HTTP-Proxy, ICQ, IMAP, IRC, LDAP, MEMCACHED, MONGODB, MS-SQL, MYSQL, NCP, NNTP, Oracle Listener,Oracle SID, Oracle, PC-Anywhere, PCNFS, POP3, POSTGRES, Radmin, RDP, Rexec, Rlogin, Rsh, RTSP, SAP/R3, SIP, SMB, SMTP, SMTP Enum, SNMP v1+v2+v3, SOCKS5, SSH (v1 and v2), SSHKEY, Subversion, Teamspeak (TS2), Telnet, VMware-Auth,VNC and XMPP.
```

[GitHub - vanhauser-thc/thc-hydra: hydra · GitHub](https://github.com/vanhauser-thc/thc-hydra)

---
## Hydra Commands 

The options we pass into Hydra depend on which service (protocol) we’re attacking. For example, if we wanted to brute force FTP with the username being `user` and a password list being `passlist.txt`, we’d use the following command:

```shell
hydra -l user -P passlist.txt ftp://MACHINE_IP
```

### SSH

```shell
hydra -l <username> -P <full path to pass> MACHINE_IP -t 4 ssh
```

| Option | Description                            |
| ------ | -------------------------------------- |
| `-l`   | specifies the (SSH) username for login |
| `-P`   | indicates a list of passwords          |
| `-t`   | sets the number of threads to spawn    |

For example :

```shell
hydra -l root -P passwords.txt 10.130.139.93 -t 4 ssh
```

will run with the following arguments:

- Hydra will use `root` as the username for `ssh`
- It will try the passwords in the `passwords.txt` file
- There will be four threads running in parallel as indicated by `-t 4`

**Or:**

```shell
hydra -L logins.txt -P passwords.txt -e nsr -t 4 -V ssh://IP_Cible
```

> [!note] `-t` — Parallel tasks
> Number of parallel connections (default **16**). Lower to `-t 4` for SSH — servers cap simultaneous connections (`MaxStartups`), so too many cause errors, false negatives, and fail2ban bans. Higher is fine for robust services like HTTP.

## Post Web Form 

We can use Hydra to brute force web forms, too. You must know which type of request it is making; GET or POST methods are commonly used. You can use your browser’s network tab (in developer tools) to see the request types or view the source code.

```shell
sudo hydra -l <username> -P <wordlist> 10.130.139.93 http-post-form "<path>:<login_credentials>:<invalid_response>"
```

| Option              | Description                                                                             |
| ------------------- | --------------------------------------------------------------------------------------- |
| `-l`                | the username for (web form) login                                                       |
| `-P`                | the password list to use                                                                |
| `http-post-form`    | The type of the form is POST                                                            |
| `<path>`            | the login page URL, for example, `login.php`                                            |
| `login_credentials` | the username and password used to log in, for example,`username=^USER^&password=^PASS^` |
| `invalid_response`  | part of the response when the login fails                                               |
| `-V`                | verbose output for every attempt                                                        |

Below is a more concrete example Hydra command to brute force a POST login form:

```shell
hydra -l <username> -P <wordlist> 10.130.139.93 http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V -t 64
```

- The login page is only `/`, i.e., the main IP address.
- The `username`  is the form field where the username is entered
- The specified username(s) will replace `^USER^`
- The `password` is the form field where the password is entered
- The provided passwords will be replacing `^PASS^`
- Finally, `F=incorrect` is a string that appears in the server reply when the login fails
- The `-V` flag shows each attempt (verbose), and `-t 64` runs 64 parallel tasks — fine for HTTP since web servers handle high concurrency (unlike SSH, where you'd lower it to `-t 4`).

On a side note, if the web server is listening on a non-default port number, you can explicitly specify the port number using `-s <port>`, for example:

```shell
hydra -l <username> -P <wordlist> 10.130.139.93 http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -s <port> -V
```


---
## Related 

- [[MOC_Tools|Tools]]

