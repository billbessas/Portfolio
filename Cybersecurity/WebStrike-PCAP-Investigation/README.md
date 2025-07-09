
# WebStrike PCAP Investigation Lab

This project is a forensic walkthrough of malicious web activity captured via a PCAP file. The goal was to identify attacker behavior, uncover exploited vulnerabilities, and determine the scope of unauthorized access through traffic analysis.

## Objective

Analyze a captured network packet file (PCAP) to:

- Reconstruct the attacker’s steps
- Identify indicators of compromise (IOCs)
- Understand the delivery and execution of a malicious web shell
- Assess whether sensitive files were accessed or exfiltrated

## Tools Used

- Wireshark  
- PCAP file (provided by CyberDefenders)  
- IP Geolocation tools  

## Documentation

- [Investigation Summary](WebStrike-Lab-Investigation_Overview.md)  
- [Video Walkthrough on YouTube](https://youtu.be/c0VsVmQ7IYs)

## MITRE ATT&CK Techniques Observed

| Tactic            | Technique                       | ID        |
|------------------|----------------------------------|-----------|
| Initial Access    | Drive-by Compromise              | T1189     |
| Execution         | Command and Scripting Interpreter | T1059     |
| Exfiltration      | Exfiltration Over HTTP           | T1041     |
| Command & Control | Application Layer Protocol       | T1071.001 |

## Key Findings

- Attacker originated from Tianjin, China  
- Malicious web shell uploaded using file extension obfuscation (`image.jpg.php`)  
- Reverse shell initiated to port `8080` using Netcat  
- Exfiltration attempt of `/etc/passwd` file via HTTP POST  

## Lessons Learned

- Importance of strict file upload validation  
- Common bypass techniques (e.g., double extensions)  
- Detecting and decoding reverse shell behavior  
- Use of metadata (User-Agent, IP) in threat intel

## Key Takeaways

- Strengthened my ability to read and interpret network traffic  
- Learned to isolate attacker behavior across sessions using Wireshark filters  
- Gained hands-on exposure to common web shell techniques and post-exploitation activity  
- Improved technical communication through written and video documentation

---

**Author:** Billy Bessas  
**Date:** July 2025  
**Lab Source:** [CyberDefenders – WebStrike](https://cyberdefenders.org/blueteam-ctf-challenges/webstrike/)  
