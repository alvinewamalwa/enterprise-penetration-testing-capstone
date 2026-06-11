# Challenge 2: Web Server Vulnerabilities

## Objective

In this challenge, I performed reconnaissance on a target web server in order to identify misconfigured directories, locate exposed resources, and retrieve a hidden file containing the Challenge 2 flag value.

My focus was on identifying directory listing vulnerabilities and exposed web application files resulting from improper server configuration.


## Step 1: Initial Setup

I accessed the target web application at:

http://10.6.6.100

I logged in using the following credentials:

- Username: admin  
- Password: password  

After logging in, I set the DVWA security level to **Low** to ensure that vulnerabilities were exposed for testing purposes.


## Step 2: Reconnaissance Using Nikto

I performed reconnaissance on the target server using Nikto in order to identify hidden directories and possible misconfigurations.

The scan revealed an accessible directory:

http://10.6.6.100/docs

This directory was publicly accessible and indicated a potential information disclosure vulnerability.


## Step 3: Directory Investigation

I navigated to the discovered directory:

http://10.6.6.100/docs

Inside this directory, I found a file named:

user_form.html

I accessed the file directly using:

http://10.6.6.100/docs/user_form.html


## Step 4: Flag Retrieval

The file contained the Challenge 2 flag value, confirming that sensitive content was exposed within a publicly accessible directory due to misconfiguration.

![Challenge 2 Flag Retrieved](../screenshots/challenge2/01-flag-retrieved.png)

## Findings

From this challenge, I identified the following security issues:

- Directory listing enabled on `/docs`
- Public exposure of sensitive files
- Lack of authentication or access control on restricted resources
- Information disclosure through web server misconfiguration

---

## Remediation

To mitigate the vulnerabilities identified, I recommend the following actions:

### 1. Disable Directory Listing
I would disable directory indexing on the web server to prevent attackers from viewing file structures.

For Apache servers, I would configure:
- `Options -Indexes`

---

### 2. Restrict Access to Sensitive Directories
I would restrict access to sensitive directories such as `/docs` using proper access control mechanisms.

This may include:
- Authentication requirements
- IP-based restrictions
- Role-based access control

---

### 3. Remove or Relocate Sensitive Files
I would ensure sensitive files are not stored in publicly accessible directories by:

- Moving them outside the web root directory
- Protecting them with authentication
- Encrypting sensitive data where necessary

---

### 4. Harden Web Server Configuration
I would harden the server by:
- Disabling unnecessary services and modules
- Restricting HTTP methods
- Applying security headers
- Performing regular configuration audits

---

### 5. Continuous Vulnerability Scanning
I would perform regular scans using tools such as:
- Nikto
- Nmap scripting engine
- OWASP ZAP

to ensure misconfigurations are detected early.

---

## Conclusion

In this challenge, I identified a misconfigured web directory that exposed sensitive files. This demonstrated how improper server configuration can lead to information disclosure.

By implementing proper access controls and disabling directory listing, I would significantly reduce the risk of exposing sensitive resources.
