# Markdown & Obsidian Cheatsheet

Quick reference for the most commonly used Markdown syntax in Obsidian.

---
## Headings

```markdown
# Heading 1

## Heading 2

### Heading 3

#### Heading 4

##### Heading 5

###### Heading 6
```

---
## Text Formatting

```markdown
**Bold text**

*Italic text*

~~Strikethrough text~~

`Inline code`
```

Result:

**Bold text**

*Italic text*

~~Strikethrough text~~

`Inline code`

---
## Lists

### Unordered List

```markdown
- Item 1
- Item 2
  - Sub-item 1
  - Sub-item 2
- Item 3
```

### Ordered List

```markdown
1. First item
2. Second item
3. Third item
```

---
## Blockquotes

```markdown
> Highlighted text, an important note, conclusion, or analysis result.
```

Nested blockquote:

```markdown
> First level
>> Second level
```

---
## Code Blocks

Generic code block:

~~~markdown
```
Code goes here
```
~~~

Code block with syntax highlighting:

~~~markdown
```python
print("Hello")
```
~~~

Cisco configuration example:

~~~markdown
```text
interface GigabitEthernet0/1
 switchport mode access
 switchport access vlan 10
```
~~~

---
## Horizontal Rule

```markdown
---
```


---
## Tables

```markdown
| First Name | Last Name |
| ---------- | --------- |
| Jean       | Durant    |
| Paul       | Dupont    |
```

Result:

| First Name | Last Name |
| ---------- | --------- |
| Jean | Durant |
| Paul | Dupont |

---
## External Links

```markdown
[University Website](https://iut-lannion.univ-rennes.fr/)
```

---
## Obsidian Internal Links

Link to another note:

```markdown
[[VLAN]]
```

Link with a custom display name:

```markdown
[[VLAN|VLAN Configuration]]
```

Link to a specific heading:

```markdown
[[VLAN#Configuration]]
```

Link to a heading with a custom display name:

```markdown
[[VLAN#Configuration|VLAN Configuration]]
```

---
## Images and Attachments

Embed an image stored inside the vault:

```markdown
![[network_topology.png]]
```

Embed an image with a specific width:

```markdown
![[network_topology.png|600]]
```

External image:

```markdown
![Description](https://example.com/image.png)
```

---
## Obsidian Embeds

Embed another note:

```markdown
![[VLAN]]
```

Embed only a section of another note:

```markdown
![[VLAN#Configuration]]
```

---
## Checkboxes

```markdown
- [ ] Task to complete
- [x] Completed task
```

---
## Escaping Markdown Characters

Use `\` when you want Markdown characters to be displayed literally.

```markdown
\# Not a heading

\* Not italic *

\> Not a blockquote
```

---
## Useful Obsidian Syntax

### Tags

```markdown
#networking
#linux
#security
```

### Comments

Comments are stored in the Markdown file but hidden in Reading View:

```markdown
%% This is an Obsidian comment. %%
```

### Callouts

```markdown
> [!NOTE]
> Additional information.

> [!TIP]
> Useful tip.

> [!WARNING]
> Important warning.
```

---
## Quick Reference

| Purpose | Syntax |
|---|---|
| Heading | `# Heading` |
| Bold | `**text**` |
| Italic | `*text*` |
| Strikethrough | `~~text~~` |
| Inline code | `` `code` `` |
| Internal link | `[[Note]]` |
| External link | `[Name](URL)` |
| Image | `![[image.png]]` |
| Blockquote | `> text` |
| Tag | `#tag` |
| Checkbox | `- [ ] Task` |
| Horizontal rule | `---` |

---
# Related 

- [[MOC_Cheatsheets|Cheatsheet]]




