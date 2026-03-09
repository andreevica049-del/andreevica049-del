Scenario
During routine network monitoring, a SOC analyst receives a PCAP file that needs to be investigated. The objective is to analyze the captured traffic using Wireshark and identify network communication patterns, DNS activity, suspicious ports, and anomalies in the traffic.
The investigation focuses on using Wireshark statistics tools and display filters toInvestigation Steps
1. Loading the PCAP file
I opened Wireshark and loaded the provided capture file:
Exercise.pcapng
 identify key indicators within the captured traffic.
<img width="974" height="548" alt="image" src="https://github.com/user-attachments/assets/5eb184f5-9297-4634-be18-07c294bcb169" />

2. Investigating Resolved Addresses
I navigated to:
Statistics → Resolved Addresses
A search for the hostname bbc revealed the associated IP address.
Result:
199.232.24.81
<img width="974" height="548" alt="image" src="https://github.com/user-attachments/assets/eb5f77db-13ba-42ef-a88f-fa48b5179dd5" />

3. Analyzing IPv4 Conversations
I opened:
Statistics → Conversations
The IPv4 tab showed the number of communication sessions between hosts.
Result:
435 IPv4 conversations
<img width="974" height="548" alt="image" src="https://github.com/user-attachments/assets/9d8e637d-2d3c-449b-8fb3-f0d0baddfaa1" />

4. Investigating Endpoints
I navigated to:
Statistics → Endpoints
Name resolution was enabled to identify the manufacturer of MAC addresses.
The device Micro-St transferred:
7474 k bytes
<img width="974" height="548" alt="image" src="https://github.com/user-attachments/assets/36f40384-3174-4029-af3a-f4df2f61283a" />

5. GeoIP Analysis
Within the IPv4 endpoint list, I sorted the entries by City.
Result:
4 IP addresses linked to Kansas City
<img width="974" height="548" alt="image" src="https://github.com/user-attachments/assets/63b77d94-5086-4ded-87de-8a0e1f184c66" />

6. ASN Investigation
I  examined the AS Organisation column and identified an IP linked with the organisation Blicnet.
Result:
188.246.82.7
<img width="974" height="548" alt="image" src="https://github.com/user-attachments/assets/12da1817-e482-4671-8138-2aa42da88441" />

7. IPv4 Statistics
I opened:
Statistics → IPv4 Statistics → All Addresses
After sorting by packet count, the most frequent destination IP was identified.
Result:
10.100.1.33
<img width="974" height="548" alt="image" src="https://github.com/user-attachments/assets/99279424-1168-4aa1-b51d-ca53a6a73ae2" />

8. DNS Analysis
Using:
Statistics → DNS
I determined the maximum DNS request-response time.
Result:
0.467897 seconds
<img width="974" height="548" alt="image" src="https://github.com/user-attachments/assets/bf90c8bc-29f3-459e-b2e7-e811a6d41cf6" />

9. HTTP Request Analysis
I  navigated to:
Statistics → HTTP → Requests
Result:
39 HTTP requests to rad.msn.com
<img width="974" height="548" alt="image" src="https://github.com/user-attachments/assets/02a4b727-1065-487d-b3dc-766d98e74736" />
<img width="974" height="548" alt="image" src="https://github.com/user-attachments/assets/5b18821e-680c-48d4-bedc-a72e2daed950" />

10. Display Filter Investigation
I applied several filters to identify specific traffic patterns.
Examples:
ip.ttl < 10
tcp.port == 4444
http.request.method == "GET"
dns.qry.type == 1
<img width="974" height="548" alt="image" src="https://github.com/user-attachments/assets/7df4cc9f-93a2-4d05-bbe3-64cc9d40dab5" />

Conclusion
The investigation demonstrated how Wireshark statistics and filtering capabilities can be used to analyze network traffic efficiently. I  revealed communication patterns, DNS activity, HTTP requests, and traffic on suspicious ports.

















