---
author: Alfeze
created: 2026-09-19
---

# PowerShell

>  *PowerShell* is a cross-platform task automation solution made up of a command-line shell, a scripting language, and a configuration management framework

---
## PowerShell Basics

To list all available cmdlets, functions, aliases, and scripts that can be executed in the current PowerShell session, we can use `Get-Command`

```powershell
Get-Command
```

> [!TIP]
> For example, if we want to display only the available commands of type “function”, we can use -CommandType "Function" as show below:

```powershell
Get-Command -CommandType "Function"
```

Another essential cmdlet to keep in our tool belt is Get-Help: it provides detailed information about cmdlets, including usage, parameters, and examples. It’s the go-to cmdlet for learning how to use PowerShell commands.

```powershell
Get-Help Get-Date
```

 >To see the examples, type: "get-help Get-Date -examples".


To make the transition easier for IT professionals, PowerShell includes aliases —which are shortcuts or alternative names for cmdlets— for many traditional Windows commands. Indispensable for users already familiar with other command-line tools, Get-Alias lists all aliases available. For example, dir is an alias for Get-ChildItem, and cd is an alias for Set-Location.

```Powershell
Get-Alias
```

---
## Find and Download Cmdlets

To search for modules (collections of cmdlets) in online repositories like the PowerShell Gallery, we can use Find-Module. Sometimes, if we don’t know the exact name of the module, it can be useful to search for modules with a similar name. We can achieve this by filtering the Name property and appending a wildcard (*) to the module’s partial name, using the following standard PowerShell syntax: Cmdlet -Property "pattern*".


```PowerShell
Find-Module -Name "PowerShell*"
```

Once identified, the modules can be downloaded and installed from the repository with Install-Module, making new cmdlets contained in the module available for use.

```PowerShell
Install-Module -Name "PowerShellGet"
```

---
## Navigating the File System and Working with Files

Similar to the dir command in Command Prompt (or ls in Unix-like systems), Get-ChildItem lists the files and directories in a location specified with the -Path parameter. It can be used to explore directories and view their contents. If no Path is specified, the cmdlet will display the content of the current working directory.

```PowerShell
 Get-ChildItem
```

>example : `Get-ChildItem -Path C:\Users`


To navigate to a different directory, we can use the Set-Location cmdlet. It changes the current directory, bringing us to the specified path, akin to the cd command in Command Prompt.

```PowerShell
Set-Location -Path ".\Documents"
```


To create an item in PowerShell, we can use New-Item. We will need to specify the path of the item and its type (whether it is a file or a directory).

```PowerShell
New-Item -Path ".\captain-cabin\captain-wardrobe" -ItemType "Directory"
```

---
## Piping , Filtering, and Sorting Data


 If you want to get a list of files in a directory and then sort them by size, you could use the following command in PowerShell:

```PowerShell
Get-ChildItem | Sort-Object Length
```

Here, `Get-ChildItem` retrieves the files (as objects), and the pipe (`|`) sends those file objects to `Sort-Object`, which then sorts them by their `Length` (size) property. This object-based approach allows for more detailed and flexible command sequences.


another example shows that objects can also be filtered by selecting properties that match (`-like`) a specified pattern:

```PowerShell
Get-ChildItem | Where-Object -Property "Name" -like "ship*"
```


The next filtering cmdlet, `Select-Object`, is used to select specific properties from objects or limit the number of objects returned. It’s useful for refining the output to show only the details one needs.

```PowerShell
Get-ChildItem | Select-Object Name,Length
```

---
## System and Network Information

### Computer Info

The `Get-ComputerInfo` cmdlet retrieves comprehensive system information, including operating system information, hardware specifications, BIOS details, and more. It provides a snapshot of the entire system configuration in a single command. Its traditional counterpart `systeminfo` retrieves only a small set of the same details.

```PowerShell
Get-ComputerInfo
```

###  Local User

Essential for managing user accounts and understanding the machine’s security configuration, `Get-LocalUser` lists all the local user accounts on the system. The default output displays, for each user, username, account status, and description.

```PowerShell
Get-LocalUser
```

### Network IP Configuration

`Get-NetIPConfiguration` provides detailed information about the network interfaces on the system, including IP addresses, DNS servers, and gateway configurations. (Similar to the traditional `ipconfig` command)

```PowerShell
Get-NetIPConfiguration
```

### Network IP Address

In case we need specific details about the IP addresses assigned to the network interfaces, the `Get-NetIPAddress` cmdlet will show details for all IP addresses configured on the system, including those that are not currently active.

```PowerShell
Get-NetIPAddress
```

---
## Real-Time System Analysis


`Get-Process` provides a detailed view of all currently running processes, including CPU and memory usage, making it a powerful tool for monitoring and troubleshooting.

```PowerShell
Get-Process
```


Similarly, `Get-Service` allows the retrieval of information about the status of services on the machine, such as which services are running, stopped, or paused. It is used extensively in troubleshooting by system administrators, but also by forensics analysts hunting for anomalous services installed on the system.

```PowerShell
Get-Service
```

Example of useful filter : 

```PowerShell
Get-Service | Where-object status -eq Running
```


To monitor active network connections, `Get-NetTCPConnection` displays current TCP connections, giving insights into both local and remote endpoints. This cmdlet is particularly handy during an incident response or malware analysis task, as it can uncover hidden backdoors or established connections towards an attacker-controlled server.

```PowerShell
Get-NetTCPConnection
```

---
## Scripting

### Executing commands on remote systems

`Invoke-Command` is essential for executing commands on remote systems, making it fundamental for system administrators, security engineers and penetration testers. `Invoke-Command` enables efficient remote management and—combining it with scripting—automation of tasks across multiple machines. It can also be used to execute payloads or commands on target systems during an engagement by penetration testers—or attackers alike.


```PowerShell
PS C:\Users\captain> Get-Help Invoke-Command -examples

NAME
   Invoke-Command 
   
SYNOPSIS
    Runs commands on local and remote computers.
    
    ------------- Example 1: Run a script on a server -------------
    
    Invoke-Command -FilePath c:\scripts\test.ps1 -ComputerName Server01
    
    The FilePath parameter specifies a script that is located on the local computer. The script runs on the remote computer and the results are returned to the local computer.

    --------- Example 2: Run a command on a remote server ---------

    Invoke-Command -ComputerName Server01 -Credential Domain01\User01 -ScriptBlock { Get-Culture }

    The ComputerName parameter specifies the name of the remote computer. The Credential parameter is used to run the command in the security context of Domain01\User01, a user who has permission to run commands. The ScriptBlock parameter specifies the command to be run on the remote computer.

    In response, PowerShell requests the password and an authentication method for the User01 account. It then runs the command on the Server01 computer and returns the result.
[...]   
```


--- 
## Related

- [[MOC_Windows|Windows]]

