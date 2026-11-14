# Incident Report: DNS Resolution Failure & Network Traffic Analysis

> *Context: Hands-on incident analysis lab completed as part of the Google Cybersecurity Professional Certificate.*

## Incident Overview
- **Analyst:** Meshack Mulei Nganda
- **Incident Timestamp:** 1:24 PM (`13:24:32.192571` per packet capture logs)
- **Affected Asset:** `yummyrecipesforme.com`
- **Protocols Analyzed:** DNS, UDP (Port 53), ICMP
- **Tools Used:** `tcpdump` (Packet Analyzer), Linux CLI

---

## 1. Incident Summary & Escalation
- At **1:24 PM** (`13:24:32.192571`), customers reported being unable to load the company website (`yummyrecipesforme.com`), encountering a **"destination port unreachable"** error after waiting for the page to load.
- To investigate the outage, I analyzed the captured `tcpdump` packet logs to pinpoint where the connection failed across the network and transport layers.

---

## 2. My Investigation & `tcpdump` Findings
Inspecting the captured `tcpdump` traffic logs, I traced the outbound DNS requests and inbound ICMP responses:

- **Outbound DNS Lookup (UDP):** I observed the first two lines of each log event showing my source host sending UDP packets to the DNS server (`203.0.113.2.domain`) to resolve the domain name.
- **DNS Flags & Record Request:** I noted a plus sign (`+`) after query ID `35084`, indicating flags associated with the UDP message, alongside the `A?` flag requesting an IPv4 **A record** to map `yummyrecipesforme.com` to an IP address.
- **Inbound Error Response (ICMP):** In the third and fourth lines of each log event, I found the DNS server returning an ICMP error packet back to my host containing the message: **`udp port 53 unreachable`**.
- **Technical Interpretation:** Because UDP port 53 is the standard port for DNS communications, this ICMP response confirmed that the DNS server was unreachable or failing to respond to domain resolution requests, preventing browsers from establishing a connection to the web server.

---

## 3. Suspected Root Causes
Based on the packet logs and port 53 behavior, I identified two primary hypotheses for why the DNS server stopped responding:
- **Denial of Service (DoS) Attack:** An attacker may have flooded the DNS server with excessive traffic, crashing the service or preventing it from responding to legitimate queries.
- **Firewall Misconfiguration:** A configuration change on the perimeter or host firewall may have inadvertently blocked network traffic on UDP port 53.

---

## 4. Next Steps & Remediation Plan
- **Verify DNS Server Health:** Confirm whether the DNS service (`named` / `systemd-resolved`) is online and functioning properly.
- **Audit Firewall Rules:** If the DNS server is healthy, inspect firewall policies and Access Control Lists (ACLs) to ensure UDP port 53 is not blocked.
- **Monitor Traffic Logs:** Review server and firewall logs around `13:24:32` for traffic anomalies to rule out a DoS flood.
