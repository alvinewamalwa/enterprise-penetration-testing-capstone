## Challenge 1: SQL Injection Exploitation

### Objective

In this challenge, I exploited an SQL injection vulnerability in a web application to extract user credentials, crack a password hash, and gain access to a remote system to retrieve a flag file.

---

### Step 1: Initial SQL Injection Test (User Enumeration)

I started by testing whether the login/input field was vulnerable to SQL injection.

**Payload used:**

1' OR '1'='1


This payload successfully bypassed authentication logic and returned a list of users from the database.


**Screenshot 1: User Enumeration Result**

![User Enumeration](../screenshots/challenge1/01-sql-injection-user-list.png)

---

### Step 2: UNION SELECT Column Discovery

I attempted to determine the number of columns in the SQL query using UNION SELECT.

**Payloads that returned errors:**

1' UNION SELECT 1,2--
1' UNION SELECT 1,2,3--

These returned SQL syntax errors because the number of columns did not match the original query structure.

I then adjusted the payload:

1' UNION SELECT 1,2#


This executed successfully and confirmed that the query uses two columns.

---

 **Screenshot 2: UNION SELECT Successful Execution**

![UNION SELECT Column Test](../screenshots/challenge1/02-union-select-column-test.png)

---

### Step 3: Extracting User Credentials

After confirming the correct column structure, I extracted usernames and password hashes from the database.

**Payload used:**

1' UNION SELECT user,password FROM users#


This returned multiple users, including Gordon Brown, along with their password hashes.



![User Credentials Extracted](../screenshots/challenge1/03-sql-injection-user-password-hashes.png)






### Step 4: Password Cracking

I identified the extracted hash as MD5 and used an online cracking tool (CrackStation.net) to recover the plaintext password.

**Hash:**


e99a18c428cb38d5f260853678922e03


**Recovered password:**

```
abc123
```

---

 **Screenshot 4: Password Cracked (CrackStation Result)**

 
![Password Cracked (MD5)](../screenshots/challenge1/04-md5-password-cracked-abc123.png)

---

### Step 5: Remote Access and Flag Retrieval

Using the cracked credentials, I logged into the target system via SSH.

* Host: 172.17.0.2
* Username: gordonb
* Password: abc123

After logging in, I navigated to the home directory and located the flag file.

I executed the following command:

```
cat hkxisx.txt
```

---

 **Screenshot 5: SSH Login and Flag Retrieval**

![SSH Login and Flag Retrieval](../screenshots/challenge1/05-ssh-login-and-challenge1-flag-retrieval.png)

---

### Result

The final flag obtained from the file is:

```
4E9f12
```

---

### Remediation

To prevent SQL injection vulnerabilities, the following controls should be implemented:

* Use parameterized queries

  
Instead of building SQL queries by directly inserting user input, applications should use parameterized queries.

What this means in practice:

Bad (vulnerable):

SELECT * FROM users WHERE username = ' " + input + " ';

Good (secure):

SELECT * FROM users WHERE username = ?;
Why it works:
The database treats user input as data only
It cannot be interpreted as SQL code
This completely breaks injection attempts like:
' OR '1'='1
UNION SELECT



* Validate and sanitize all user inputs


This ensures only expected data is accepted by the application.

Types:
a) Validation (strict rules)

Example:

Username → only letters and numbers
Age → only integers
Email → must match email format

b) Sanitization (cleaning input)
Removes or escapes dangerous characters like:
'
"
--
;
Why it matters:

Even if SQL injection is possible, malformed input is rejected before reaching the database layer.





* Disable detailed SQL error messages in production



This means database accounts should only have the minimum permissions required.

Example:

A web application user should only have:

SELECT
INSERT (if needed)

NOT:

DROP TABLE
DELETE ALL
ADMIN privileges
Why it matters:

If SQL injection happens:

attacker damage is limited
they cannot destroy or fully control the database



* Apply least privilege to database accounts


A WAF sits between users and the web application and filters malicious requests.

What it detects:
SQL injection patterns
suspicious payloads like:
' OR 1=1
UNION SELECT
stacked queries ; DROP TABLE
Why it helps:
Blocks attacks before they reach your server
Provides real-time protection even if code is vulnerable
Example tools:
Cloudflare WAF
AWS WAF
ModSecurity (open source)




* Implement Web Application Firewalls (WAF)


Applications should never show raw SQL errors to users.

Bad example (unsafe):
You have an error in your SQL syntax near 'OR 1=1'
Good example:
Invalid request. Please try again.
Why this matters:

Verbose errors give attackers:

table structure hints
database type
column information
debugging clues

This makes exploitation much easier.



* Logging and Monitoring


Log all failed login attempts
Monitor repeated injection patterns
Alert on suspicious queries


*Use ORM Frameworks

ORMs (Object Relational Mappers) like:

Hibernate (Java)
SQLAlchemy (Python)
Sequelize (Node.js)

They automatically reduce raw SQL usage.






---

### Conclusion

This challenge demonstrated how SQL injection can be exploited to extract sensitive data, escalate access, and retrieve system-level information. Proper input handling and secure database practices are essential to prevent such attacks.

