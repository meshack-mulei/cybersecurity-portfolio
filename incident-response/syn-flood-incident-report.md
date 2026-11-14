# Incident Report: SYN Flood DoS Attack & TCP Connection Analysis

> *Context: Hands-on incident analysis lab completed as part of the Google Cybersecurity Professional Certificate.*

## Incident Overview
- **Analyst:** Meshack Mulei Nganda
- **Incident Type:** Denial of Service (DoS) / Connection Timeout
- **Affected Asset:** Corporate Web Server
- **Protocols Analyzed:** TCP (SYN, SYN-ACK, ACK), IP
- **Tools Used:** Network Traffic Analyzer / Packet Sniffer, Linux CLI

---

## 1. Incident Summary & Attack Identification
- **User Impact:** Website visitors reported experiencing connection timeout errors when attempting to reach the web server.
- **Attack Identification:** Analyzing the network traffic logs, I identified that the website's connection timeouts were caused by a **Denial of Service (DoS) attack—specifically a SYN flood**. 
- **Log Findings:** The traffic logs showed that the web server stopped responding because it was overwhelmed by a massive flood of simultaneous SYN packet requests.

---

## 2. TCP Three-Way Handshake & Attack Mechanics
To understand how the SYN flood disrupted service, I analyzed the standard connection process versus malicious resource exhaustion:

- **Normal TCP Three-Way Handshake:**
  1. **SYN:** A SYN (synchronize) packet is sent from the source client to the destination server, requesting to initiate a connection.
  2. **SYN-ACK:** The destination server replies to the source with a SYN-ACK packet to accept the request, reserving memory and resources for that specific connection.
  3. **ACK:** A final ACK (acknowledgment) packet is sent from the source back to the destination, confirming the connection is established.

- **SYN Flood Malicious Impact:**
  * When a malicious actor floods the server with a massive volume of SYN packets all at once, it rapidly consumes all of the server's available resources and connection queue memory.
  * Because the server allocates resources awaiting the final ACK for each fake or incomplete handshake, no server resources remain for legitimate visitor connection requests.

---

## 3. Conclusion & Log Interpretation
- **Server Status:** The packet logs confirmed that the web server became completely overwhelmed and unable to process incoming visitor SYN requests.
- **Result:** Because the connection backlog queue was maxed out, legitimate users trying to visit the site received a **connection timeout message**.
