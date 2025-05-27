# WebStrike - Lab 1: Investigating a Suspicious File Upload

A forensic walkthrough of malicious web activity captured via PCAP, analyzed using Wireshark to identify attacker behavior, uncover vulnerabilities, and determine the scope of unauthorized access.


## Demo Video


[Watch the Demo Video](https://youtu.be/c0VsVmQ7IYs)



## Lab Scenario

A suspicious file was discovered on a company web server. The Development team flagged the anomaly, and a PCAP file of the relevant network traffic was captured. Your task: trace the attacker's activity, understand how the file was uploaded, and determine what (if anything) was exfiltrated.


## Tools Used

- **Wireshark** (for full traffic analysis)
- **IP Geolocation** services
- **PCAP** file provided via Cyber Defenders
- Markdown and screenshots for documentation

---

## Summary of Findings

| Question | Answer |
|------------|-----------|
| **1. City of attack origin** | Tianjin |
| **2. Attacker's User-Agent** | `Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0` |
| **3. Name of malicious web shell** | `image.jpg.php` |
| **4. Upload directory** | `/reviews/uploads/` |
| **5. Outbound communication port** | `8080` |
| **6. File targeted for exfiltration** | `/etc/passwd` |

---

## Walkthrough

### 1. Identifying the geographical origin of the attack helps in implementing geo-blocking measures and analyzing threat intelligence. From which city did the attack originate?

- Used **Wireshark** to analyze IPv4 endpoints  
  ![Endpoints in Wireshark](./images/endpoints-view.png)

- Identified `117.11.88.124` as external attacker IP  
- Verified geolocation using [ipgeolocation.io](https://ipgeolocation.io)  
  ![GeoIP Lookup Screenshot](./images/ip-geo.png)

---

### 2. Knowing the attacker's User-Agent assists in creating robust filtering rules. What's the attacker's Full User-Agent?

- Followed the HTTP stream from the attacker:
  ![HTTP Stream](./images/user-agent-stream.png)

- Full User-Agent revealed:  
  `Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0`

---
