---
author: Alfeze
created: 2026-09-24
---

# Hashcat

>**Hashcat** is an open-source, ultra-fast password recovery tool optimized for GPU acceleration to crack hashes offline. It is used in security audits and pentesting to recover plaintext passwords using dictionary, mask, and rule-based attacks

---
## Basic Syntax

```bash
hashcat -m <hash_type> -a <attack_mode> hashfile wordlist
```

- `-m <hash_type>` specifies the hash-type in numeric format. For example, `-m 1000` is for NTLM. Check the official documentation (`man hashcat`) and [example page(opens in new tab)](https://hashcat.net/wiki/doku.php?id=example_hashes) to find the hash type code to use.
- `-a <attack_mode>` specifies the attack-mode. For example, `-a 0` is for straight, i.e., trying one password from the wordlist after the other.
- `hashfile` is the file containing the hash you want to crack.
- `wordlist` is the security word list you want to use in your attack.

For example,

```bash
hashcat -m 3200 -a 0 hash.txt /usr/share/wordlists/rockyou.txt
```

>Will treat the hash as Bcrypt and try the passwords in the `rockyou.txt` file.

---
## Cracking Password Hashes with Hashcat

### Hash Identification

We inspect the target hash stored in `hash2.txt`:

![[Pasted image 20260924205436.png]]

> **Output**: **9eb7ee7f551d2f0ac684981bd1f1e2fa4a37590199636753efe614d4db30e8e1**

- **Length:** 64 hexadecimal characters ($64 \times 4\text{ bits} = 256\text{ bits}$).
- **Algorithm:** This length typically indicates a **SHA-256** hash.

### Attack Execution 

To crack this hash, we run Hashcat using a dictionary attack against the `rockyou.txt` wordlist:

```bash
hashcat -m 1400 -a 0 hash2.txt /usr/share/wordlists/rockyou.txt
```

**Command Breakdown :** 
- **`hashcat`**: Invokes the cracking utility.
- **`-m 1400`**: Specifies the hash mode (`1400` corresponds to SHA-256).
- **`-a 0`**: Selects the attack mode (`0` = Straight / Dictionary attack; tests entries sequentially).
- **`hash2.txt`**: The target file containing the hash to crack.
- **`/usr/share/wordlists/rockyou.txt`**: Path to the reference wordlist.

### Output Analysis & Results

![[Pasted image 20260924205735.png]]

![[Pasted image 20260924205847.png]]

Once launched, Hashcat loads the hash and tests the wordlist:

- **Cracked Credentials:** `9eb7ee7f551d2f0ac684981bd1f1e2fa4a37590199636753efe614d4db30e8e1:halloween`
- **Cleartext Password:** **`halloween`**

---
## Quick Alternative: Online Lookups & Precomputed Databases

Before launching a resource-heavy offline attack with Hashcat or John, checking precomputed databases (rainbow tables and indexed wordlists) online can often recover the plaintext instantly:

* **[CrackStation](https://crackstation.net/):** Industry reference for instantly looking up unsalted hashes (MD5, SHA-1, SHA-256, NTLM) using massive precomputed lookup tables.

* **[Hashes.com](https://hashes.com/en/decrypt/hash):** High-coverage multi-algorithm decryption platform supporting bulk lookups and a broad range of hash types (MD5, SHA-1, NTLM, SHA-256, SHA-512, bcrypt). **Multi-Format Hash Decryptors (e.g., MD5Decrypt):** Useful for quickly checking common web hashes, MySQL, or CMS-specific formats like WordPress.

* **[FrameIP Cisco Type 7 Decryptor](https://www.frameip.com/):** Instantly decrypts Cisco Type 7 passwords (which use a weak, reversible XOR cipher rather than a true cryptographic hash).

> **Security & OPSEC Note:** Never paste real-world corporate hashes or sensitive customer data into third-party online crackers, as these platforms log submissions. Keep online lookups strictly for CTFs and training labs.

---
## Related 

- [[MOC_Tools|Tools]]
