---
author: Alfeze
created: 2026-09-19
---

# Windows Command Line

> The default command-line interpreter (cmd.exe) in Windows  systems, used to interact directly with the operating system, execute administrative utilities, and automate tasks through batch scripts.
---
## Basic cmd Command

`set` to check your path from the command line.

```cmd
C:\>set
```

`ver` command to determine the operating system version.

```cmd
C:\>ver
```

`systeminfo` command to list various information about the system such as OS information, system details, processor and memory.

```text
C:\>systeminfo
```


> [!TIP]
> you can pipe it through `more` if the output is too long. Then, you can view it page after page by pressing the space bar button. To demonstrate this, try running `driverquery` and compare it with running `driverquery | more`. In the latter, you can display the output page by page and you can exit it using `CTRL + C`.

## Networking Commands

 `netstat` command to displays current network connections and listening ports.

```cmd
C:\>netstat
```

If you are curious about the other options, you can run `netstat -h`, where `-h` displays the help page. We opted for the following options:

- `-a` displays all established connections and listening ports
- `-b` shows the program associated with each listening port and established connection
- `-o` reveals the process ID (PID) associated with the connection
- `-n` uses a numerical form for addresses and port numbers

We combine these four options and execute the `netstat -abon` command.

## Process Management

 We can list the running processes using `tasklist`

```cmd
C:\>tasklist
```

we want to search for tasks related to `sshd.exe`, we can do that with the command :
`tasklist /FI "imagename eq sshd.exe"`

With the process ID (PID) known, we can terminate any task using `taskkill /PID target_pid`. For example, if we want to kill the process with PID `4567`, we would issue the command `taskkill /PID 4567`.

--- 
## Related

- [[MOC_Windows|Windows]]


