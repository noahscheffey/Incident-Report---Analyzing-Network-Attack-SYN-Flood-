<h1>Incident Response: Mitigating a SYN Flood Attack on a Client Website</h1>

<h2>Incident Overview</h2>

The client's website, a critical platform for advertising sales and promotions, experienced a service outage, impacting employees' ability to assist customers. Initial investigation revealed that the outage was caused by a SYN flood attack, a type of denial-of-service (DoS) attack. This attack overwhelmed the web server, preventing legitimate employee access and hindering business operations.

<h2>Initial Response and Analysis</h2>

I detected the web server outage via an automated alert. My initial response involved:

1. Verifying the outage and its impact on employee access.

2. Performing packet capture and analysis using Wireshark to identify the root cause.

3. The Wireshark analysis, as shown below, revealed a high volume of TCP SYN requests originating from an unknown IP address, indicating a SYN flood attack. The server was unable to process these requests, leading to connection timeouts for legitimate users.

<h2>Containment and Mitigation</h2>

To contain the attack and mitigate its immediate impact, I took the following actions:

1. Temporarily took the affected web server offline to allow it to recover.

2. Configured the firewall to block the attacking IP address.

3. I recognized that IP blocking alone was not a long-term solution due to the possibility of IP address spoofing.

<h2>Escalation and Recommendations</h2>

I immediately escalated the issue to my manager, providing a detailed explanation of the attack, its impact, and the limitations of the initial mitigation strategy. I recommended implementing the following preventative measures:

1. SYN Flood Protection: Implement SYN cookies or rate limiting on the firewall and web server to prevent future SYN flood attacks.

2. Intrusion Detection System (IDS): Deploy an IDS to detect and alert on suspicious network traffic patterns, including those indicative of DoS attacks.

3. I also emphasized the importance of a proactive security posture and the need for ongoing monitoring and maintenance of security systems.

<h2>Wireshark Log Analysis</h2>

The Wireshark capture provided valuable insights into the attack:

<img src="https://i.imgur.com/glhZcwQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/914fNyW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/OrA4B9h.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

<h2>Initial Observations:</h2>

The image shows a high volume of SYN packets, which is characteristic of a SYN flood attack.

**Normal Traffic:** The log initially shows normal web traffic, with employees (e.g., IP address 198.51.100.23) accessing the web server (192.0.2.1). This includes the standard TCP three-way handshake (SYN, SYN-ACK, ACK) and HTTP GET requests for web pages.

**Attack Traffic:** The attack is characterized by a high volume of SYN packets originating from a single, unknown IP address (203.0.113.0). These SYN packets flood the server, preventing it from responding to legitimate connection requests.

**Impact:** As the attack progresses, the server becomes overwhelmed, resulting in:

1. HTTP/1.1 504 Gateway Time-out errors for legitimate users, indicating the server is unresponsive.

2. [RST, ACK] packets being sent to legitimate users, indicating connection resets.

**Conclusion:** The Wireshark log clearly demonstrates a SYN flood attack, where the attacker exploits the TCP handshake process to exhaust server resources and cause a denial of service.
 
<h2>Conclusion</h2>

This incident highlights the significant impact a SYN flood attack can have on a web server's availability and the importance of implementing robust mitigation and prevention strategies. I am prepared to implement and manage solutions such as SYN cookies and rate limiting to enhance the organization's security posture, ensure business continuity, and minimize the risk of future attacks.
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
