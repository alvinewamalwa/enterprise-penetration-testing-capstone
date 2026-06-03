# SMB Enumeration Challenge 3

## Overview

This challenge focuses on identifying SMB services, enumerating available shares, and locating sensitive files accessible through misconfigured permissions. The goal was to retrieve the flag stored within one of the SMB shares.

---

## Step 1: Identifying SMB Services

To begin, I scanned the network for hosts with SMB services running. Since SMB operates on ports 139 and 445, I specifically targeted these ports.

```
nmap -p 139,445 --open 10.6.6.0/24
```

The scan revealed that the host **10.6.6.23** had both ports open, indicating active SMB services.

### Screenshot 1: SMB Port Scan Results

![Description](screenshots/challenge3/step1_smb_scan.png)

---

## Step 2: Enumerating SMB Shares

Next, I enumerated the SMB shares on the target to determine which ones were accessible without authentication.

```
smbclient -L 10.6.6.23 -N
```

The following shares were discovered:

* homes (not accessible)
* workfiles (accessible)
* print$ (accessible)
* IPC$ (accessible)

I attempted anonymous access to each share to verify permissions.

### Screenshot 2: SMB Share Enumeration

![SMB Shares](screenshots/challenge3/step2_share_enum.png)

---

## Step 3: Exploring Shares and Locating the File

After identifying accessible shares, I systematically explored them to locate the file containing the Challenge 3 code.

I began with the `print$` share because it contained multiple subdirectories, making it a strong candidate for hidden or misplaced files.

### Screenshot 3: Exploration of print$ Share

![Share Exploration](screenshots/challenge3/step3_exploration.png)




Most directories contained no useful files, suggesting they were system-related. I continued navigating until I found a directory named `OTHER`.

Inside the `OTHER` directory, I discovered a file named `taxes.txt`.


I downloaded the file using `get` command and opened it locally, revealing the flag.

### Screenshot 4: File Discovery and Flag

![Flag Found](screenshots/challenge3/step4_flag.png)

---

## Final Answer

* **Share:** print$
* **File:** taxes.txt
* **Flag:** A9!15wa2

---

## SMB Security Remediations

### 1. Disable Anonymous Access

Anonymous access allowed unrestricted entry into SMB shares, making it possible to retrieve sensitive files without authentication.

This should be mitigated by enforcing authentication on all SMB shares. Requiring valid credentials ensures that only authorized users can access shared resources.

---

### 2. Enforce Proper Access Control

The presence of sensitive data in an accessible share highlights poor permission management.

Applying the principle of least privilege ensures users only access what they need. Sensitive files should be restricted to specific users or groups with strict permissions.

---

### 3. Restrict Administrative Shares

The `print$` share is typically meant for administrative use, yet it was accessible anonymously.

Such shares should be restricted to authorized administrators only, using both share-level and network-level controls.

---

### 4. Remove Unnecessary Shares

Unused or non-essential shares increase the attack surface.

Regular audits should be conducted to identify and disable unnecessary shares, reducing potential entry points for attackers.

---

### 5. Enable Logging and Monitoring

The ability to access and download files without detection indicates a lack of monitoring.

Enabling SMB logging and integrating logs into a monitoring system allows detection of suspicious activities such as unauthorized access attempts.

---

### 6. Use Secure SMB Versions

Older SMB versions are vulnerable to multiple exploits.

SMBv1 should be disabled, and only secure versions such as SMBv2 or SMBv3 should be used to improve overall system security.

---

## Conclusion

Through structured enumeration and careful exploration of SMB shares, I identified a misconfigured share that exposed sensitive data. This demonstrates how improper access controls can lead to serious security risks and reinforces the importance of securing SMB services.

