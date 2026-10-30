---
author: Alfeze
created: 2026-10-28
---

# Introduction to SIEM

> Security Information and Event Management system (SIEM) is the core security solution that a SOC analyst uses in the security operations center.

---

Security Information and Event Management (SIEM) is a security solution that collects logs from various types of log sources, standardizes their format into a consistent one, correlates them, and detects malicious activities using detection rules.


![[image.png|389]]

## Features of SIEM

- **Centralized Log Collection**
- **Normalization of Logs**
- **Correlation of Logs**
- **Real-time Alerting**
- **Dashboards and Reporting**

--- 
## Log Ingestion

All these logs provide a wealth of information and can help identify security issues. Each SIEM solution has its own way of ingesting the logs. Some common methods used by these SIEM solutions are explained below:

1. Agent / Forwarder  

These SIEM solutions provide a lightweight tool called an agent (forwarder by Splunk) that gets installed on the Endpoint. It is configured to capture and send all the important logs to the SIEM server.

2. **Syslog**  

Syslog is a widely used protocol to collect data from various systems like web servers, databases, etc., and send real-time data to the centralized destination.

3. **Manual Upload**  

Some SIEM solutions, like Splunk, ELK, etc., allow users to ingest offline data for quick analysis. Once the data is ingested, it is normalized and made available for analysis.

4. **Port-Forwarding**  

SIEM solutions can also be configured to listen on a certain port, and then the endpoints forward the data to the SIEM instance on the listening port.

---
