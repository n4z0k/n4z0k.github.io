---
author: Alfeze
created: 2026-09-20
---

# Wireshark Basics

> Wireshark is an open-source, cross-platform network packet analyser tool capable of sniffing and investigating live traffic and inspecting packet captures (PCAP). It is commonly used as one of the best packet analysis tools. 

---
## Tool Overview

The picture below shows Wireshark's main window. 

![[Pasted image 20260920114333.png]]


![[Pasted image 20260920114439.png]]


| Packet List Pane     | Summary of each packet (source and destination addresses, protocol, and packet info). You can click on the list to choose a packet for further investigation. Once you select a packet, the details will appear in the other panels. |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Packet Details Panel | Detailed protocol breakdown of the selected packet.                                                                                                                                                                                  |
| Packet Bytes Pane    | Hex and decoded ASCII representation of the selected packet. It highlights the packet field depending on the clicked section in the details pane.                                                                                    |

- **Colouring Packets** : You can create custom colour rules to spot events of interest by using display filters

![[Pasted image 20260920114834.png]]


- **Traffic Sniffing** : You can use the blue **"shark button"** to start network sniffing (capturing traffic), the red button will stop the sniffing, and the green button will restart the sniffing process. The status bar will also provide the used sniffing interface and the number of collected packets.

![[Pasted image 20260920115206.png]]


- **Merge PCAP Files** : Wireshark can combine two pcap files into one single file. You can use the **"File --> Merge"** menu path to merge a pcap with the processed one. When you choose the second file, Wireshark will show the total number of packets in the selected file. Once you click "open", it will merge the existing pcap file with the chosen one and create a new pcap file. Note that you need to save the "merged" pcap file before working on it.

![[Merge.gif]]


- **View File Details** : Knowing the file details is helpful. Especially when working with multiple pcap files, sometimes you will need to know and recall the file details (File hash, capture time, capture file comments, interface and statistics) to identify the file, classify and prioritise it. You can view the details by following "**Statistics --> Capture File Properties"** or by clicking the **"pcap icon located on the left bottom"**.

![[View.gif]]


---
## Packet Navigation

- **Go to Packet**: Packet numbers do not only help to count the total number of packets or make it easier to find/investigate specific packets. This feature not only navigates between packets up and down; it also provides in-frame packet tracking and finds the next packet in the particular part of the conversation. You can use the **"Go"** menu and toolbar to view specific packets.

![[Go_To_Packet.gif]]


- **Find Packets** :  You can use the **"Edit --> Find Packet"** menu to make a search inside the packets for a particular event of interest. This helps analysts and administrators to find specific intrusion patterns or failure traces.

![[Find.gif]]


- **Mark Packets** : Marking packets is another helpful functionality for analysts. You can find/point to a specific packet for further investigation by marking it. It helps analysts point to an event of interest or export particular packets from the capture. You can use the **"Edit"** or the **"right-click"** menu to mark/unmark packets.

![[Mark.gif]]


- **Packet Comments**: Marking packets is another helpful functionality for analysts. You can find/point to a specific packet for further investigation by marking it. It helps analysts point to an event of interest or export particular packets from the capture. You can use the **"Edit"** or the **"right-click"** menu to mark/unmark packets.

![[Comment.gif]]


- **Export Packets**:This functionality helps analysts share the only suspicious packages (decided scope)

![[Export.gif]]


- **Export Objects (Files)** : Wireshark can extract files transferred through the wire. For a security analyst, it is vital to discover shared files and save them for further investigation. Exporting objects are available only for selected protocol's streams (DICOM, HTTP, IMF, SMB and TFTP).

![[export_object.gif]]


- **Time Display Format**: The common usage is using the UTC Time Display Format for a better view. You can use the **"View --> Time Display Format"** menu to change the time display format.

![[Time.gif]]


![[Pasted image 20260920121245.png]]


- **Expert Info** : Wireshark also detects specific states of protocols to help analysts easily spot possible anomalies and problems. Note that these are only suggestions, and there is always a chance of having false positives/negatives. Expert info can provide a group of categories in three different severities. Details are shown in the table below.

![[Pasted image 20260920130301.png]]


---
## Packet Filtering

- **Apply as Filter** :This is the most basic way of filtering traffic. While investigating a capture file, you can click on the field you want to filter and use the "right-click menu" or **"Analyse** **--> Apply as Filter"** menu to filter the specific value. Note that the number of total and displayed packets are always shown on the status bar.

![[Apply_Filter.gif]]


- **Conversation Filter** : "Conversation Filter" option helps you view only the related packets and hide the rest of the packets easily. You can use the"right-click menu" or "**Analyse --> Conversation Filter**" menu to filter conversations.

![[Conversation_Filter.gif]]


- **Colourise Conversation**: "Conversation Filter" option helps you view only the related packets and hide the rest of the packets easily. You can use the"right-click menu" or "**Analyse --> Conversation Filter**" menu to filter conversations.

![[Colourise.gif]]


- **Prepare as Filter** : Similar to "Apply as Filter", this option helps analysts create display filters using the "right-click" menu. However, unlike the previous one, this model doesn't apply the filters after the choice. It adds the required query to the pane and waits for the execution command (enter) or another chosen filtering option by using the **".. and/or.."** from the "right-click menu".

![[prepare_filter.gif]]


- **Apply as Column**: By default, the packet list pane provides basic information about each packet. You can use the "right-click menu" or "**Analyse --> Apply as Column**" menu to add columns to the packet list pane.

![[Column.gif]]


- **Follow Stream**: By default, the packet list pane provides basic information about each packet. You can use the "right-click menu" or "**Analyse --> Apply as Column**" menu to add columns to the packet list pane.

![[Stream.gif]]


----

## Related

- [[MOC_Tools|Tools]]
