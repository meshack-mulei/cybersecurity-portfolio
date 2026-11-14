# Incident Report: HTTP Traffic Analysis, Malicious Redirect, & Brute-Force Remediation

> *Context: Hands-on incident analysis lab completed as part of the Google Cybersecurity Professional Certificate.*

## Incident Overview
- **Analyst:** Meshack Mulei Nganda
- **Incident Type:** Web Compromise / Malicious Redirection & Brute-Force Attack
- **Affected Asset:** `yummyrecipesforme.com` (Corporate Web Server & Admin Portal)
- **Protocols Analyzed:** HTTP (Hypertext Transfer Protocol)
- **Tools Used:** Sandbox Environment, `tcpdump` (Packet Analyzer), Source Code Analysis

---

## 1. Section 1: Network Protocol Involved
- **Primary Protocol:** Hypertext Transfer Protocol (HTTP).
- **Justification:** Because the issue involved accessing the web server for `yummyrecipesforme.com` via web browsers, the traffic relied on HTTP at the application layer. 
Analyzing the `tcpdump` capture confirmed that browser requests and server responses were communicated over HTTP.

---

## 2. Section 2: Incident Documentation & Investigation
- **Initial User Reports:** Customers contacted the helpdesk reporting that when visiting `yummyrecipesforme.com`, they were prompted to download and run a file that claimed to provide access to new recipes, after which their personal computers operated slowly. 
Concurrently, the website owner attempted to log into the web server, but noticed they were locked out of their administrator account.

- **Sandbox Investigation:** 
  1. A secure sandbox environment was utilized to open the website without impacting the company network.
  2. Network traffic was captured using `tcpdump` while interacting with the website. During this process, a prompt appeared offering a file download claiming to provide access to free recipes. Accepting the download redirected the browser to a fake website (`greatrecipesforme.com`).
  3. Inspection of the `tcpdump` logs revealed that the browser initially requested the IP address for the legitimate `yummyrecipesforme.com` website over HTTP. 
Once the connection was established, executing the downloaded file triggered a sudden change in network traffic as the browser requested a new IP address for the malicious `greatrecipesforme.com` URL.

- **Root Cause Discovery:** Source code analysis of the website and downloaded files revealed that an attacker had manipulated the website code to prompt users to download a malicious file disguised as a browser update. 
Because the website owner was locked out of their admin account, it was determined that the attacker utilized a **brute-force attack** to compromise the administrator credentials, allowing them to alter the site and execute the malicious file deployment.

---

## 3. Section 3: Recommendations & Remediations for Brute-Force Attacks
To secure the administrative portal and prevent future brute-force compromises, the following controls are recommended:
- **Disallow Reuse of Previous Passwords:** Prevent attackers from exploiting default or weak passwords by enforcing robust password history policies.
- **Enforce Regular Password Updates:** Require periodic password updates to minimize the window of vulnerability if credentials are exposed.
- **Implement Two-Factor Authentication (2FA):** Require authentication via a password combined with a one-time passcode (OTP) sent to a trusted device or email, ensuring that even if an account password is brute-forced, access is denied without the second factor.
