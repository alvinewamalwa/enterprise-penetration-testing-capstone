## PCAP Analysis and Web Resource Extraction

## Overview

In this challenge, I analyzed a packet capture file (SA.pcap) using Wireshark to identify the target system, trace HTTP communication, and locate a hidden file containing the Challenge 4 code. The analysis involved examining packet-level details, identifying active hosts, and reconstructing a web-accessible URL from captured traffic.

## Step 1: Identifying Network Communication at Packet Level

I began by opening the packet capture file in Wireshark:

wireshark /home/kali/OTHER/SA.pcap

Before applying any filters, I first inspected raw packet-level data to understand the communication structure between hosts.

At this stage, I focused on Frame details, which included:

Ethernet II layer (MAC addresses)
IPv4 layer (source and destination IP addresses)
TCP layer (source and destination ports)

This helped establish the relationship between communicating devices before filtering application-layer traffic.


Screenshot 1:![Packet Frame Details](../screenshots/challenge4/frame_details.png)


This screenshot shows:

Ethernet II source and destination MAC addresses
IPv4 source and destination IP addresses
TCP source and destination ports
Full packet structure under Frame 1

From this, I was able to confirm the presence of active communication between internal hosts in the network.



## Step 2: Filtering HTTP Traffic

After identifying the communication structure, I filtered the capture to focus on HTTP traffic only:

`http`

This was necessary to isolate web-based requests and ignore lower-level protocol noise such as TCP acknowledgements and background traffic.

Screenshot 2: ![HTTP Traffic Analysis](../screenshots/challenge4/http_filtered.png)

This screenshot shows:

HTTP GET requests
Source and destination IP addresses
Packet length information
Web requests in the Info column

At this stage, I identified that the client system (10.6.6.1) was sending requests to the target server (10.6.6.14).

Step 3: Identifying Target System and HTTP Requests

From the filtered HTTP traffic, I analyzed request patterns:

Source IP: 10.6.6.1 (client system initiating requests)
Destination IP: 10.6.6.14 (web server responding to requests)

I confirmed the target system by observing that 10.6.6.14 consistently served HTTP responses to incoming GET requests.

I then examined the Info column and extracted HTTP request paths.

Key observed endpoints included:

/database-offline.php
/test/
/includes
/passwords
/webservices/
/data


## Step 4: Identifying the Suspicious Directory

Among all discovered endpoints, I focused on /data as the most relevant directory.

This decision was based on the following reasoning:

It is commonly used to store structured backend data
It is not part of the frontend interface (unlike /styles or /javascript)
It typically contains files such as XML, JSON, or database exports
It appeared more likely to expose sensitive information compared to other directories

This made /data the most promising candidate for further investigation.



## Step 5: Accessing the Hidden File in Browser

Using the discovered directory from the PCAP, I constructed the following URL:

http://10.6.6.14/data/accounts.xml

Opening this URL in a browser revealed an XML file containing structured employee records.

This was not an HTML page, but an XML document, meaning it stored hierarchical data rather than rendering a user interface.

Screenshot 3: ![Flag File in Browser](../screenshots/challenge4/flag_browser.png)

This screenshot shows the XML file opened in the browser. It contains multiple employee records, including the required flag!


Inside the XML file, I located the required record.
The challenge specifically instructed that the flag is located in the Signature field of Employee ID “0”.

Final Answer

The Challenge 4 code is:

zz90014x

Security Remediations
1. Restrict Access to Sensitive Directories

The /data directory was publicly accessible, exposing structured internal data. This should be prevented by restricting direct web access and storing sensitive files outside the web root.

2. Disable Directory Exposure and Enumeration

If directory listing or direct file access is enabled, attackers can easily discover hidden resources. Web servers should be configured to disable directory indexing to prevent enumeration of internal structure.

3. Enforce Proper Access Controls

Sensitive files such as accounts.xml should not be accessible without authentication. Role-based access control should be enforced to ensure only authorized users can access backend data.

4. Separate Backend Data from Public Web Content

Storing structured data inside web-accessible directories increases exposure risk. Backend data should be stored in protected storage locations or accessed via secure APIs instead of direct file access.

5. Enable Logging and Monitoring

The ability to access sensitive files without restriction indicates lack of monitoring. Web server logs should be enabled and reviewed regularly to detect unusual access patterns or reconnaissance activity.

6. Secure Web Application Design

Direct exposure of structured XML data indicates poor application design. Modern applications should avoid exposing raw data files and instead serve data through controlled application logic with validation and authentication.

## Conclusion

Through packet-level inspection and HTTP traffic analysis, I identified the target system, reconstructed a hidden URL, and accessed a sensitive XML file containing the Challenge 4 code. This demonstrated how improper web configuration and exposed backend resources can lead to information disclosure through network traffic analysis.
