---
author: Alfeze
created: 2026-09-19
---

# Post Installation

>Essential setup steps, localization settings, and quick environment adjustments for fresh Linux installations.
---
## Keyboard Layout Configuration

Switch keyboard layout to French AZERTY for the current session.

```bash
setxkbmap fr
```

> [!tip] QWERTY Key Mapping Reference
> If your physical keyboard is currently recognized as QWERTY:
> - Press **Q** to type `a`
> - Press **,** (comma) to type `m`
> - Press **A** to type `q`
> - Press **Z** to type `w`

### Permanent Configuration (Debian / Kali Linux)

Reconfigure console and GUI keyboard settings permanently across reboots.

```bash
sudo dpkg-reconfigure keyboard-configuration
```

### Steps in the Configuration Wizard

1. **Keyboard Model:** `Generic 105-key PC` (default)
2. **Country of Origin:** `French`
3. **Keyboard Layout:** `French` (or `French - French (alternative)`)
4. **AltGr Key:** `The default for the keyboard layout`
5. **Compose Key:** `No compose key`

 Restart the keyboard setup service to apply changes immediately without rebooting.

```bash
sudo systemctl restart keyboard-setup
```

---
## Linux - Timezone & Clock Configuration

 Set the timezone to Paris and enable automatic network time synchronization (NTP).

```bash
sudo timedatectl set-timezone Europe/Paris
sudo timedatectl set-ntp true
```

> [!tip] Verification
> Run `timedatectl` to confirm the local time, timezone (`Europe/Paris`), and NTP active status.


---
## Kali Undercover

>**Kali Undercover** is a feature in Kali Linux that changes your desktop interface to look like Windows.

```bash
kali-undercover
```


---
## Related

- [[MOC_Linux|Linux]]



