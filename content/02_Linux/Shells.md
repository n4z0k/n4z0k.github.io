---
author: Alfeze
created: 2026-09-19
---

# Shells

> A command-line interpreter that provides an interface between the user and the operating system kernel, executing commands and managing scripts.

---
## Current Shell

Multiple shells are installed in different Linux distributions. To see which shell you are using, type the following command:

```bash
echo $SHELL
```

## Available Shells

You can also list down the available shells in your Linux OS. The file /etc/shells contains all the installed shells on a Linux system. You can list down the available shells in your Linux OS by typing cat /etc/shells in the terminal

```bash
cat /etc/shells
```

## Switch Shell

```bash
zsh
```

If you want to permanently change your default shell, you can use the command:  `chsh -s /usr/bin/zsh`. This will make this shell as the default shell for your terminal.

---
## Related

- [[MOC_Linux|Linux]]



