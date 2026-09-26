---
author: Alfeze
created: 2026-09-26
---
# Integrity_Checking

>Checking a downloaded file's hash against the vendor's official signature ensures the file was neither corrupted during transfer nor tampered with (supply chain / MITM attack).

---
## Step-by-Step Verification Workflow

- Create a clean directory to keep the files isolated, then navigate into it:

```bash
mkdir ~/integrity-check && cd ~/integrity-check
```

- Download the target `.iso` image into this folder.

- Obtain the official hash provided on the vendor's website. Create a verification file `check.txt` containing the exact hash, followed by **two spaces**, and the exact file name:

Example : 

```bash
echo "a1b2c3d4e5f6...hash_string_here...  debian-12.0.0-amd64-netinst.iso" > check.txt
```

>**Syntax Rule:** The standard format strictly requires **two spaces** (or a tab) between the hash and the file name: `<hash> <filename>`


- Run the verification command 

Execute the hash utility matching the algorithm with the `-c` (`--check`) flag:

```bash
sha256sum -c check.txt
```


- **For SHA-256:**

```bash
sha256sum -c check.txt
```

- **For SHA-512:**

```bash
sha512sum -c check.txt
```

- **For MD5:**

```bash
md5sum -c check.txt
```

---
## File Hashing & Integrity with Powershell

>The `Get-FileHash` cmdlet computes the cryptographic hash of a file using a specified hash algorithm. It is essential in **incident response**, **threat hunting**, and **malware analysis** to verify file integrity, detect unauthorized tampering, and check samples against known threat intelligence indicators (IoCs).


### Syntax & Basic Usage

>By default, `Get-FileHash` uses the **SHA256** algorithm:

```powershell
# Compute default SHA256 hash
Get-FileHash -Path .\ship-flag.txt
```

### Supported Algorithms

>You can specify alternate algorithms using the `-Algorithm` parameter:

```powershell
# Supported: SHA1, SHA256, SHA384, SHA512, MD5
Get-FileHash -Path .\ship-flag.txt -Algorithm MD5
Get-FileHash -Path .\ship-flag.txt -Algorithm SHA512
```

### Practical One-Liners

```powershell
# Extract raw hash string only (ideal for copy-pasting to VirusTotal)
(Get-FileHash -Path .\ship-flag.txt).Hash

# Direct equality check against a known malicious/legitimate hash
(Get-FileHash -Path .\ship-flag.txt).Hash -eq "EXPECTED_OR_KNOWN_HASH_VALUE"

# Hash all files recursively in a directory for baseline comparison
Get-ChildItem -Path C:\TargetDir -Recurse -File | Get-FileHash -Algorithm SHA256 | Select-Object Path, Hash
```

Example :

![[Pasted image 20260919132042.png|700]]

> [!TIP]
> When downloading rolling or weekly images (e.g., `kali-linux-2026-W38-installer-amd64.iso`), never compare the hash against the main download page UI, which statically reflects the quarterly milestone release (`2026.2`). Retrieve the checksum directly from the matching directory index (`kali-weekly/SHA256SUMS`) — comparing the hash in PowerShell via `(Get-FileHash ...).Hash -eq "<checksum>"` returns `True`, verifying file integrity without false corruption alarms caused by the avalanche effect.

[ Kali Linux's base-images repository](https://cdimage.kali.org/)

---
## Related

- [[MOC_Security|Security]]


