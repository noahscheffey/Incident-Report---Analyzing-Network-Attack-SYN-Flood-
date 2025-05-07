<h1> Incident Report - Analyzing Network Attack (SYN Flood) </h1>

<h2>Web Server Denial of Service Mitigation</h2>

I detected a web server outage via an automated alert. Initial troubleshooting revealed a connection timeout. I performed packet capture and analysis, identifying a high volume of TCP SYN requests originating from an unknown IP address. This indicated a likely SYN flood attack, overwhelming the server and preventing legitimate access.

My response included:

Temporarily taking the server offline to facilitate recovery.

Configuring the firewall to block the attacking IP address.

I recognized the limitations of IP blocking against spoofed addresses and immediately escalated the issue to my manager, providing a detailed explanation of the attack and recommending further preventative measures.

<h2>Wireshark Capture</h2>

<img src="https://i.imgur.com/OQGyjwJ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/914fNyW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/OrA4B9h.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

<h2>Initial Observations:</h2>

The log excerpt begins at packet number 47, corresponding to 3.144 seconds into the capture. This shows the initial stages of normal website traffic. The source IP address 198.51.100.23 is identified as belonging to an employee's computer, while 192.0.2.1 is the web server. 

Normal web browsing involves a TCP three-way handshake followed by HTTP requests. Packets 47-49 illustrate this handshake: the employee's computer initiates the connection with a SYN packet (47), the server responds with SYN, ACK (48), and the employee completes the handshake with ACK (49). Packet 50 shows a normal HTTP GET request for /sales.html from the employee, and packet 51 shows the server's HTTP/1.1 200 OK response. 

<h2>Identifying the Attack:</h2>
Starting around packet 52, I observe a different source IP address: 203.0.113.0. This address sends a SYN packet (52) to the web server. The server responds with a SYN, ACK (53), as it would in a normal connection. Crucially, the attacking IP (203.0.113.0) sends an ACK (54), appearing to complete the handshake. However, the attacking IP continues to send more SYN packets (e.g., packet 57, 59, 61), even though it has seemingly completed a handshake. This is suspicious. 

<h2>Progression and Impact:</h2>
The log shows a mix of normal employee traffic (highlighted in green) and the attacker's repeated SYN packets (highlighted in red). As the attack progresses, the number of SYN packets from 203.0.113.0 increases significantly. I see evidence of the server becoming overloaded. Packets 73, 77, and 80 show errors: HTTP/1.1 504 Gateway Time-out: The web server is taking too long to respond to legitimate employee requests. [RST, ACK]: The server is resetting connections with employees, likely because it cannot allocate resources.  From around packet 125 onward, the log is dominated by the attacker's SYN packets. The server is overwhelmed and effectively stops responding to legitimate employee traffic. The web server is unable to open a new connection to new visitors who receive a connection timeout message. The repeated SYN packets from a single source (203.0.113.0) indicate a direct Denial of Service (DoS) attack, specifically a SYN flood.
 
<h2>Conclusion</h2>

The log demonstrates a SYN flood attack. The attacker is sending a large number of SYN packets to the web server, exhausting its connection resources and preventing it from responding to legitimate employee requests. The server is eventually overwhelmed, leading to timeouts and connection resets for legitimate users.

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
