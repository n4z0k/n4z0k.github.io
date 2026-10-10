---
author: Alfeze
created: 2026-08-29
---
# Boot System

> The `boot system` command specifies which Cisco IOS image the device should load during the next boot. The IOS image is typically stored in flash memory.

```text
S1# dir flash:
→ Displays files and directories stored in flash memory

S1# configure terminal

S1(config)# boot system flash:<path>/<ios-image.bin>
→ Specifies the IOS image to load at the next boot

S1(config)# end

S1# show boot
→ Displays the current boot settings and BOOT environment variable

S1# copy running-config startup-config
→ Saves the boot configuration for the next restart

S1# reload
→ Restarts the switch and loads the configured IOS image
```
## Example

```text
S1# dir flash:

S1# configure terminal
S1(config)# boot system flash:/c2960-lanbasek9-mz.150-2.SE/c2960-lanbasek9-mz.150-2.SE.bin
S1(config)# end

S1# show boot
S1# copy running-config startup-config
```

---
## Related

- [[MOC_Networking|Networking]]
