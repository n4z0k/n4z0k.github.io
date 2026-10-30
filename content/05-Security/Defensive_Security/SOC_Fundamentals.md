---
author: Alfeze
created: 2026-10-28
---

# SOC Fundamentals

> A **SOC** (**S**ecurity **O**perations **C**enter) is a dedicated facility operated by a specialized security team. This team aims to continuously monitor an organization’s network and resources and identify suspicious activity to prevent damage. This team works 24 hours a day, seven days a week.
---
## Purpose

![[Pasted image 20261028145923.png|401]]


**Detection :**
- **Detect vulnerabilities**
- **Detect unauthorized activity**
- **Detect policy violations**
- **Detect intrusions**

**Response :**
- **Support with the incident response**:

---
##  Three Pillars of a SOC

There are three pillars of a SOC. With all these pillars, a SOC team becomes mature and efficiently detects and responds to different incidents. These pillars are **People**, **Process**, and **Technology**.
![[Pasted image 20261028150017.png|421]]

---
## People 

The **People** are known as the SOC team.

![[Pasted image 20261028151044.png|602]]



---

## Process

#### Alert Triage

The alert triage is the basis of the SOC team. The first response to any alert is to perform the triage. The triage is focused on analyzing the specific alert. This determines the severity of the alert and helps us prioritize it. The alert triage is all about answering the 5 Ws. What are these 5 Ws?

![[Pasted image 20261028152457.png|435]]


**What: Activity that triggered the alert ?**
**Where: Destination host IP ?**
**When: Time of the activity ?**
**Why: Reason for the activity ? Intended / Malicious**
**Who: Source host name ?**

Example : 
`Alert: Malware detected on Host: GEORGE PC`

| 5 Ws   | Answers                                                                                                                                                                                                                       |
| ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| What?  | A malicious file was detected on one of the hosts inside the organization’s network.                                                                                                                                          |
| When?  | The file was detected at 13:20 on June 5, 2024.                                                                                                                                                                               |
| Where? | The file was detected in the directory of the host: "GEORGE PC".                                                                                                                                                              |
| Who?   | The file was detected for the user George.                                                                                                                                                                                    |
| Why?   | After the investigation, it was found that the file was downloaded from a pirated software-selling website. The investigation with the user revealed that they downloaded the file as they wanted to use a software for free. |
#### Reporting

The detected harmful alerts need to be escalated to higher-level analysts for a timely response and resolution. These alerts are escalated as tickets and assigned to the relevant people. The report should discuss all the 5 Ws along with a thorough analysis, and screenshots should be used as evidence of the activity.

#### Incident Response and Forensics

Sometimes, the reported detections point to highly malicious activities that are critical. In these scenarios, high-level teams initiate an incident response. A few times, a detailed forensics activity also needs to be performed. This forensic activity aims to determine the incident’s root cause by analyzing the artifacts from a system or network.

---
## Technology

The **Technology** portion in the SOC pillars refers to the security solutions. These security solutions efficiently minimize the SOC team's manual effort to detect and respond to threats.

Security solutions centralize all the information of the devices or applications present in the network and automate the detection and response capabilities.

- **SIEM:** Security Information and Event Management (SIEM) is a popular tool used in almost every SOC environment. This tool collects logs from various network devices, referred to as log sources. Detection rules are configured in the SIEM solution, which contains logic to identify suspicious activity. The SIEM solution provides us with the detections after correlating them with multiple log sources and alerts us in case of a match with any of the rules. Modern SIEM solutions surpass this rule based detection analysis, providing us with user behavior analytics and threat intelligence capability. Machine learning algorithms support this to enhance the detection capabilities.

> [!NOTE]
> The SIEM solution only provides the **Detection** capabilities in a SOC environment.

- **EDR:** Endpoint Detection and Response (EDR) provides the SOC team with detailed real-time and historical visibility of the devices’ activities. It operates on the endpoint level and can carry out automated responses. EDR has extensive detection capabilities for endpoints, allowing you to investigate them in detail and respond with a few clicks.

- **Firewall:** A firewall functions purely for network security and acts as a barrier between your internal and external networks (such as the Internet). It monitors incoming and outgoing network traffic and filters any unauthorized traffic. The firewall also has some detection rules deployed, which help us identify and block suspicious traffic before it reaches the internal network.

Several other security solutions play unique roles in a SOC environment, such as Antivirus, EPP, IDS/IPS, XDR, SOAR, and more. The decision on what Technology to deploy in the SOC comes after careful consideration of the threat surface and the available resources in the organization.

---
## Related

- [[MOC_Security|Security]]
