# 🚫 Broken Access Control (IDOR) – TryHackMe Walkthrough

## 📝 Overview

This challenge demonstrates **Insecure Direct Object Reference (IDOR)**, a common form of **Broken Access Control** vulnerability. IDOR occurs when user-supplied input is not properly validated, allowing attackers to directly manipulate object references (like user IDs, account numbers, or file paths) to gain unauthorized access to data.

---

## 🎯 Lab Objective

* Exploit an **IDOR vulnerability** to access other users’ private notes.
* Retrieve a sensitive flag from another user’s account.

---

## 🛠 Steps to Exploit

### ✅ 1. Understanding the Vulnerability

* Logged in as a normal user with the following credentials:

  ```
  Username: noot
  Password: test1234
  ```

* After logging in, user-specific notes were displayed.

---

### ✅ 2. Manipulating the Object Reference

* Observed the request URL or parameters after logging in.
* Modified the parameter (e.g., user ID or note ID) to access other users’ data.

---

### ✅ 3. Retrieving Other Users’ Notes

* Successfully accessed another user’s notes by manipulating the object reference.

* **Flag Retrieved:**

  ```
  flag{fivefourthree}
  ```

---

## ✅ Key Takeaways

* **Root Cause:** Application relied on client-side input to determine which resource to serve without verifying proper authorization.

* **Impact:**

  * Exposure of sensitive user data.
  * Unauthorized access to private resources.

* **Mitigation:**

  * Always enforce **server-side authorization checks**.
  * Use **indirect object references** (e.g., mapping IDs to random tokens).
  * Perform proper input validation and session handling.

---

## 🗺 MITRE ATT\&CK Mapping

| **Technique**                         | **ID**                                              | **Description**                                                               |
| ------------------------------------- | --------------------------------------------------- | ----------------------------------------------------------------------------- |
| **Accessing Sensitive Data via IDOR** | [T1190](https://attack.mitre.org/techniques/T1190/) | Exploiting vulnerable web applications to access unauthorized resources.      |
| **Valid Accounts**                    | [T1078](https://attack.mitre.org/techniques/T1078/) | Attackers use valid credentials and manipulate object references to escalate. |