---
author: Alfeze
created: 2026-10-09
---

# Burp Suite

> Burp Suite is a Java-based framework designed to serve as a comprehensive solution for conducting web application penetration testing. It has become the industry standard tool for hands-on security assessments of web and mobile applications, including those that rely on application programming interfaces (APIs).

---
## Features of Burp Community 

- **Proxy**: The Burp Proxy is the most renowned aspect of Burp Suite. It enables interception and modification of requests and responses while interacting with web applications.

- **Repeater**: Another well-known feature. Repeater allows for capturing, modifying, and resending the same request multiple times. This functionality is particularly useful when crafting payloads through trial and error (e.g., in SQLi - Structured Query Language Injection) or testing the functionality of an endpoint for vulnerabilities. [TryHackMe \| Burp Suite: Repeater](https://tryhackme.com/room/burpsuiterepeater)

- **Intruder**: Despite rate limitations in Burp Suite Community, Intruder allows for spraying endpoints with requests. It is commonly utilized for brute-force attacks or fuzzing endpoints. [TryHackMe \| Burp Suite: Intruder](https://tryhackme.com/room/burpsuiteintruder)

- **Decoder**: Decoder offers a valuable service for data transformation. It can decode captured information or encode payloads before sending them to the target. While alternative services exist for this purpose, leveraging Decoder within Burp Suite can be highly efficient. [TryHackMe \| Burp Suite: Other Modules](https://tryhackme.com/room/burpsuiteom)

- **Comparer**: As the name suggests, Comparer enables the comparison of two pieces of data at either the word or byte level. While not exclusive to Burp Suite, the ability to send potentially large data segments directly to a comparison tool with a single keyboard shortcut significantly accelerates the process. [TryHackMe \| Burp Suite: Other Modules](https://tryhackme.com/room/burpsuiteom)

- **Sequencer**: Sequencer is typically employed when assessing the randomness of tokens, such as session cookie values or other supposedly randomly generated data. If the algorithm used for generating these values lacks secure randomness, it can expose avenues for devastating attacks. [TryHackMe \| Burp Suite: Other Modules](https://tryhackme.com/room/burpsuiteom)

---
## Navigation 

Burp Suite provides keyboard shortcuts for quick navigation to key tabs. By default, the following shortcuts are available:

| Shortcut         | Tab          |
| ---------------- | ------------ |
| Ctrl + Shift + D | Dashboard    |
| Ctrl + Shift + T | Target tab   |
| Ctrl + Shift + P | Proxy tab    |
| Ctrl + Shift + I | Intruder tab |
| Ctrl + Shift + R | Repeater tab |

---
## FoxyProxy extension

Here are the steps to configure the Burp Suite Proxy with FoxyProxy:

**Install FoxyProxy**: Download and install the FoxyProxy Basic extension.

**Access FoxyProxy Options**: Once installed, a button will appear at the top right of the Firefox browser. Click on the FoxyProxy button to access the FoxyProxy options pop-up.

![[Pasted image 20261010093515.png]]

**Create Burp Proxy Configuration**: In the FoxyProxy options pop-up, click the Options button. This will open a new browser tab with the FoxyProxy configurations. Click the Add button to create a new proxy configuration.

![[Pasted image 20261010094126.png]]

**Add Proxy Details**: On the "Add Proxy" page, fill in the following values:

Title: `Burp` (or any preferred name)
Proxy IP:  `127.0.0.1`
Port:`8080`

![[Pasted image 20261010094238.png]]

**Save Configuration**: Click Save to save the Burp Proxy configuration.

**Activate Proxy Configuration**: Click on the FoxyProxy icon at the top-right of the Firefox browser and select the Burp configuration. This will redirect your browser traffic through `127.0.0.1:8080`. Note that Burp Suite must be running for your browser to make requests when this configuration is activated.

![[Pasted image 20261010094305.png]]

**Enable Proxy Intercept in Burp Suite**: Switch to Burp Suite and ensure that Intercept is turned on in the Proxy tab.

![[Pasted image 20261010094404.png]]


---

## Scoping and Targeting 

![[scoping.gif]]



![[Pasted image 20261010112934.png]] 

---
## Proxying HTTPS

When intercepting HTTP traffic, we may encounter an issue when navigating to sites with TLS enabled. For example, when accessing a site like https://google.com/, we may receive an error indicating that the PortSwigger Certificate Authority (CA) is not authorised to secure the connection. This happens because the browser does not trust the certificate presented by Burp Suite.

![[Pasted image 20261010113803.png]]


![[cert.gif]]

By completing these steps, we have added the PortSwigger CA certificate to our list of trusted certificate authorities. Now, we should be able to visit any TLS-enabled site without encountering the certificate error.

[Installing Burp's CA certificate - PortSwigger](https://portswigger.net/burp/documentation/desktop/external-browser-config/certificate)

---
## Example Attack 

**Walkthrough:**

![[Pasted image 20261010114809.png]]


Try typing: 

```javascript
<script>alert("Succ3ssful XSS")</script>
```
 
 into the "Contact Email" field. You should find that there is a client-side filter in place which prevents you from adding any special characters that aren't allowed in email addresses:

![[XSS.gif]]  

Let's focus on simply bypassing the filter for now.

First, make sure that your Burp Proxy is active and that intercept is on.

Now, enter some legitimate data into the support form. For example: "pentester@example.thm" as an email address, and "Test Attack" as a query.

Submit the form — the request should be intercepted by the proxy.

With the request captured in the proxy, we can now change the email field to be our very simple payload from above: 

`<script>alert("Succ3ssful XSS")</script>`

 After pasting in the payload, we need to select it, then URL encode it with the Ctrl + U shortcut to make it safe to send. This process is shown in the GIF below:

![[XSS_Proxy.gif]]

Finally, press the "Forward" button to send the request.

You should find an alert box from the site indicating a successful XSS attack!

![[Pasted image 20261010115206.png]]


---

## Related 

- [[MOC_Tools|Tools]]

