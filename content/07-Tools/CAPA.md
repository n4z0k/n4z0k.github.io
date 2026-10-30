---
author: Alfeze
created: 2026-10-29
---

# CAPA

> CAPA (Common Analysis Platform for Artifacts) is a tool developed by the FireEye Mandiant team. It is designed to **identify the capabilities** present in executable files like Portable Executables (PE), ELF binaries, .NET modules, shellcode, and even sandbox reports.

---
## Introduction 

When analyzing potentially malicious software is that we risk our machine or environment being compromised when we run it unless we have a **sandbox** (Hardware or software dedicated for isolating untrusted applications or services from critical system resources and other programs to prevent harmful actions or malware from negatively affecting the system) or a completely isolated environment where we can test all we want. 

There are two types of analysis: 

- **Dynamic analysis** : The process of analyzing malware by running it in a controlled environment like a **sandbox**.

- **Static analysis :** The process of analyzing malware without executing it, but in a controlled environment using a tool like CAPA.

CAPA analyzing the file and applying a set of rules that **describe common behaviours**, allowing it to determine what the **program is capable of doing**, such as **network communication**, **file manipulation**, **process injection**, and many more.

The beauty of CAPA is that it encapsulates years of reverse engineering knowledge into an automated tool, making it accessible even to those who may not be experts in reverse engineering. This can be incredibly useful for analysts and security professionals, allowing them to quickly understand potentially malicious software's functionality without manually reverse engineering the code.

This tool is particularly useful in malware analysis and threat hunting, where understanding a binary's capabilities is crucial for incident response and defensive measures.

---
## Tool Overview

```powerhsell
capa.exe .\cryptbot.bin
```

| Option                | Description                            | Sample Syntax                 |
| --------------------- | -------------------------------------- | ----------------------------- |
| `-h` or `--help`      | Show this help message and exit.       | `capa -h`                     |
| `-v` or `--verbose`   | Enable verbose result document.        | `capa.exe .\cryptbot.bin -v`  |
| `-vv` or `--vverbose` | Enable a very verbose result document. | `capa.exe .\cryptbot.bin -vv` |

---

## CAPA Web Explorer

[capa Explorer Web](https://mandiant.github.io/capa/explorer/#/)