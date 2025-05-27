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
- The attack originated from Tianjin, China
- **Answer** - Tianjin

---

### 2. Knowing the attacker's User-Agent assists in creating robust filtering rules. What's the attacker's Full User-Agent?

- Followed the HTTP stream from the attacker:
  ![HTTP Stream](./images/user-agent-stream.png)

- Revealed full User-Agent:  
  `Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0`
- **Answer** - `Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0`
---

### 3. We need to determine if any vulnerabilities were exploited. What is the name of the malicious web shell that was successfully uploaded?

- Applied filter:  
  ```wireshark
  ip.src == 117.11.88.124 and http.request.method == "POST"
- Two upload attempts observed:
  - First failed: `image.php`
  - Second succeeded: `image.jpg.php`
    ![Successful Upload](./images/upload-success.png)
- The attacker successfully bypassed the server's validation by appending .jpg to the filename
- **Answer** - image.jpg.php
---

### 4. Identifying the directory where uploaded files are stored is crucial for locating the vulnerable page and removing any malicious files. Which directory is used by the website to store the uploaded files?

- File was successfully uploaded via `upload.php`
- Reviewed the server's HTTP response behavior
- Server returned `303 See Other`, redirecting to `/reviews/uploads/`
  ![303 Redirect](./images/303.png)
- Followed this redirect, the uploaded file (`image.jpg.php`) was accessible at that path
- The website stores uploaded files in the `/reviews/uploads/` directory
- **Answer** - `/reviews/uploads/`

