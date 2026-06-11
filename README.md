### Cisco Penetration Testing Challenge

## Overview
This repository documents how I conducted a complete penetration test starting with reconnaissance and then launching exploits against vulnerabilities that I had discovered. Finally, I proposed remediation for the exploits.

This exercise is in the form of a cybersecurity capture the flag exercise. I used my ethical hacking skills to locate files that contain flag values. And then reported the flag values that I had found as part of the exercise.

---

## Assessment Context

The lab environment provided access to systems within the following networks:

* 10.6.6.0/24
* 172.17.0.0/24

The work performed includes reconnaissance, service enumeration, traffic analysis, and extraction of sensitive information from vulnerable systems.

---

## Scope

The analysis covered:

* Web application vulnerability testing
* Web server enumeration and directory discovery
* SMB share enumeration and access analysis
* Packet capture (PCAP) inspection and HTTP traffic reconstruction

---

## Repository Structure

All detailed findings are documented inside the `reports/` directory. The reports include all the vulnerabilities discovered, successful exploits, and remediation steps to protect vulnerable systems.

Each challenge is presented as a separate report containing:

* Step-by-step methodology
* Commands executed during analysis
* Observations and reasoning
* Identified vulnerabilities
* Remediation recommendations

---


Every report is accompanied by carefully captured screenshots that:

* Show the output of key commands
* Illustrate how tools were used during analysis
* Provide visual proof of findings and extracted data

These screenshots are included to make the workflow clear, traceable, and easy to follow for anyone reviewing the work.

---

## Key Highlights

* Identification of SQL injection vulnerability in a web application
* Discovery of exposed directories through enumeration
* Access to misconfigured SMB shares with anonymous permissions
* Reconstruction of hidden endpoints from network traffic
* Extraction of sensitive data from PCAP analysis

---

## Reports

Detailed walkthroughs are available in the `reports/` directory:

* Challenge 1: Web Application Analysis
* Challenge 2: Web Server Enumeration
* Challenge 3: SMB Share Analysis
* Challenge 4: PCAP Traffic Analysis

Each report provides a complete breakdown of the approach, findings, and supporting evidence.

---

## Disclaimer

This project was conducted in a controlled lab environment for educational and training purposes. All activities were performed on intentionally vulnerable systems within isolated networks.

The techniques demonstrated are intended solely for learning and defensive security awareness. No unauthorized systems or real-world targets were accessed during this work.


## Conclusion

This project demonstrates practical understanding of how vulnerabilities can be identified through systematic analysis of systems, services, and network traffic.

The focus is not only on identifying weaknesses, but also on understanding how they can be mitigated through proper configuration, access control, and monitoring.

