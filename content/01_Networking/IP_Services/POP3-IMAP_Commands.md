---
author: Alfeze
created: 2026-09-19
---
# POP3

> The Post Office Protocol (POP) is designed for retrieving email messages from a mail server by downloading them directly to a local client. By default, downloaded messages are stored locally and removed from the server, making it best suited for single-device offline access rather than multi-device synchronization. The default POP3 port is 110 (unencrypted), and the secure port is 995 (POP3S, encrypted via SSL/TLS).

## POP3 commands

- `USER <username>` identifies the user
- `PASS <password>` provides the user’s password
- `STAT`  requests the number of messages and total size
- `LIST`  lists all messages and their sizes
- `RETR <message_number>` retrieves the specified message
- `DELE <message_number>` marks a message for deletion
- `QUIT` ends the POP3 session applying changes, such as deletions

---
# IMAP

>The Internet Message Access Protocol (IMAP) allows clients to access and manage email messages directly on the mail server in real time, keeping mail state synchronized across multiple devices. The default IMAP port is 143 (unencrypted / STARTTLS), and the secure port is 993 (IMAPS, encrypted via SSL/TLS).

## IMAP commands 

- `LOGIN <username> <password> ` authenticates the user
- `SELECT <mailbox>` selects the mailbox folder to work with
- `FETCH <mail_number> <data_item_name>` Example `fetch 3 body[]` to fetch message number 3, header and body.
- `MOVE <sequence_set> <mailbox>` moves the specified messages to another mailbox
- `COPY <sequence_set> <data_item_name>` copies the specified messages to another mailbox
- `LOGOUT` logs out

---
## Related

- [[MOC_Networking|Networking]]
