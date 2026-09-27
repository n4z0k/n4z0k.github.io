---
author: Alfeze
created: 2026-09-24
---

# John The Ripper

> John the Ripper is a free and open-source password-cracking tool. It can crack passwords stored in various formats, including hashes, passwords, and encrypted private keys. It can be used to test passwords' security and recover lost passwords. The most popular extended version of John the Ripper : **Jumbo John**.

---
## John Basic Syntax

```bash
john [options] [file path]
```

- `john`: Invokes the John the Ripper program
- `[options]`: Specifies the options you want to use
- `[file path]`: The file containing the hash you’re trying to crack; if it’s in the same directory, you won’t need to name a path, just the file.

Main options  :

- **`--format=crypt`**: instructs John to use the Unix `crypt` hashing format, commonly used for passwords in Linux distributions like Debian. (Run `john --list=formats` to see all available formats.)

- **`--incremental`**: tells John to perform an incremental brute-force attack.

- **`--min-length=` and `--max-length=`**: set the minimum and maximum password length to generate.

Example Usage : 

```bash
john --format=crypt --incremental --min-length=2 --max-length=5 shadowCible
```

---
## Automatic Cracking

```bash
john --wordlist=[path to wordlist] [path to file]
```


- `--wordlist=`: Specifies using wordlist mode, reading from the file that you supply in the provided path
- `[path to wordlist]`: The path to the wordlist you’re using, as described in the previous task

**Example Usage:**

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt hash_to_crack.txt
```

---
## Identifying Hashes

We can use tools to identify the hash and then set John to a specific format.

 Online hash identifier
- [Hash Type Identifier - Identify unknown hashes](https://hashes.com/en/tools/hash_identifier)

Python tool that is super easy to use and will tell you what different types of hashes the one you enter is likely to be, giving you more options if the first one fails.
- [Kali Linux / Packages / hash-identifier · GitLab](https://gitlab.com/kalilinux/packages/hash-identifier/-/tree/kali/master)

To use hash-identifier, you can use `wget` or `curl` to download the Python file `hash-id.py` from its GitLab. Then, launch it with `python3 hash-id.py` and enter the hash you’re trying to identify. It will give you a list of the most probable formats. These two steps are shown in the terminal below.

![[Pasted image 20260925194210.png]]

![[Pasted image 20260925194510.png]]

---
## Format-Specific Cracking

>Once you have identified the hash that you’re dealing with, you can tell John to use it while cracking the provided hash using the following syntax:


```bash
john --format=[format] --wordlist=[path to wordlist] [path to file]
```


- `--format=`: This is the flag to tell John that you’re giving it a hash of a specific format and to use the following format to crack it

- `[format]`: The format that the hash is in

Example Usage : 

```bash
john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hash_to_crack.txt
```

---
## Cracking Windows Authentication Hashes - NTHash / NTLM

>You can acquire **NTHash/NTLM** hashes by dumping the **SAM** (Security Account Manager) database on a Windows machine, using a tool like **Mimikatz**, or using the Active Directory database: `NTDS.dit`.

Example of NTLM Hash : `5460C85BD858A11475115D2DD3A82333`

We store this hash in the **ntlm.txt** file. 

```bash
john --format=nt --wordlist=/usr/share/wordlists/rockyou.txt ntlm.txt
```

The cracked value of this password is : `mushroom`

---
## Cracking Hashes from /etc/shadow

### Unshadowing

>John can be very particular about the formats it needs data in to be able to work with it; for this reason, to crack **/etc/shadow** passwords, you must combine it with the **/etc/passwd** file for John to understand the data it’s being given. To do this, we use a tool built into the John suite of tools called **unshadow**. The basic syntax of unshadow is as follows:

```bash
unshadow [path to passwd] [path to shadow]
```


- `unshadow` : Invokes the unshadow tool
- `[path to passwd]` : The file that contains the copy of the `/etc/passwd` file
- `[path to shadow]`: The file that contains the copy of the `/etc/shadow` file

Example Usage : 

```bash
unshadow local_passwd local_shadow > unshadowed.txt
```

### Cracking 

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt --format=sha512crypt unshadowed.txt
```

---
## Single Crack Mode 

>An ultra-fast cracking mode that doesn't use an external wordlist; instead, John generates password candidates by applying mutation rules directly to the username prepended to the hash (`user:hash`)


```bash
john --single --format=[format] [path to file]
```

- `--single`: This flag lets John know you want to use the single hash-cracking mode
- `--format=[format]`: As always, it is vital to identify the proper format.

**Example Usage:**

```bash
john --single --format=raw-sha256 hashes.txt
```

> [!A Note on File Formats in Single Crack Mode:]
>If you’re cracking hashes in single crack mode, you need to change the file format that you’re feeding John for it to understand what data to create a wordlist from. You do this by prepending the hash with the username that the hash belongs to, so according to the above example, we would change the file `hashes.txt`

- **From** `1efee03cdcb96d90ad48ccc7b8666033`
- **To** `mike:1efee03cdcb96d90ad48ccc7b8666033`

Example : 

Assuming that the user it belongs to is called “**Joker**”

![[Pasted image 20260925214229.png]]

--- 
## Custom Rules

> Custom rules are defined in the `john.conf` file. This file can be found in `/opt/john/john.conf` 

- *Wiki* : [John the Ripper - wordlist rules syntax](https://www.openwall.com/john/doc/RULES.shtml)

The first line:
`[List.Rules:THMRules]` is used to define the name of your rule; this is what you will use to call your custom rule a John argument.

We could then call this custom rule a John argument using the  `--rule=PoloPassword` flag.

As a full command: `john --wordlist=[path to wordlist] --rule=PoloPassword [path to file]`

---
## Cracking Password Protected Zip Files

### Zip2John 

>Similarly to the `unshadow` tool we used previously, we will use the `zip2john` tool to convert the Zip file into a hash format that John can understand and hopefully crack. 

The primary usage is like this:

```bash
zip2john [options] [zip file] > [output file]
```

- `[options]`: Allows you to pass specific checksum options to `zip2john`; this shouldn’t often be necessary
- `[zip file]`: The path to the Zip file you wish to get the hash of
- `>`: This redirects the output from this command to another file
- `[output file]`: This is the file that will store the output

Example Usage : 

```bash
zip2john zipfile.zip > zip_hash.txt
```

### Cracking 

We’re then able to take the file we output from `zip2john` in our example use case, `zip_hash.txt`, and, as we did with `unshadow`, feed it directly into John as we have made the input specifically for it.

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt zip_hash.txt
```

![[Pasted image 20260925220052.png]]

---
## Cracking a Password-Protected RAR Archive 

### Rar2John 

>Almost identical to the `zip2john` tool, we will use the `rar2john` tool to convert the RAR file into a hash format that John can understand. 

The basic syntax is as follows:

```bash
rar2john [rar file] > [output file]
```

- `rar2john`: Invokes the `rar2john` tool
- `[rar file]`: The path to the RAR file you wish to get the hash of
- `>`: This redirects the output of this command to another file
- `[output file]`: This is the file that will store the output from the command

Example Usage :

```bash
/opt/john/rar2john rarfile.rar > rar_hash.txt
```

### Cracking 

Once again, we can take the file we output from `rar2john` in our example use case, `rar_hash.txt`, and feed it directly into John as we did with `zip2john`.

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt rar_hash.txt
```

![[Pasted image 20260925220738.png]]

---
## Cracking SSH Key Passwords

### SSH2John 

>`ssh2john` converts the `id_rsa` private key, which is used to log in to the SSH session, into a hash format that John can work with.

> [!NOTE]
>  If you don’t have `ssh2john` installed, you can use `ssh2john.py`, located in the `/opt/john/ssh2john.py`.


```bash
ssh2john [id_rsa private key file] > [output file]
```


- `ssh2john`: Invokes the `ssh2john` tool
- `[id_rsa private key file]`: The path to the id_rsa file you wish to get the hash of
- `>`: This is the output director. We’re using it to redirect the output from this command to another file.
- `[output file]`: This is the file that will store the output from

Example Usage : 

```bash
/opt/john/ssh2john.py id_rsa > id_rsa_hash.txt
```
### Cracking 

For the final time, we’re feeding the file we output from ssh2john, which in our example use case is called `id_rsa_hash.txt` and, as we did with `rar2john`, we can use this seamlessly with John:

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt id_rsa_hash.txt
```


![[Pasted image 20260925221807.png]]

![[Pasted image 20260925221926.png]]

---
## Related 

- [[MOC_Tools|Tools]]
