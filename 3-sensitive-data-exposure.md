# 🔒 Sensitive Data Exposure – TryHackMe Walkthrough

## 📝 Overview

This challenge demonstrates how sensitive data can be accidentally exposed when developers store flat-file databases (e.g., **SQLite**) directly on the web server. Attackers can download these files, extract confidential information, and exploit it to gain unauthorized access.

---

## 🎯 Lab Objective

* Identify a publicly accessible directory containing sensitive data.
* Extract credentials from a **SQLite database**.
* Crack the admin password hash and log in to obtain the flag.

---

## 🛠 Steps to Exploit

### ✅ 1. Locate the Sensitive Directory

* **Clue:** The developer left a note pointing to a directory.
* **Answer:**

  ```
  /assets
  ```

### ✅ 2. Identify the Exposed Database

* Navigating to `/assets` revealed a suspicious file:

  ```
  webapp.db
  ```

### ✅ 3. Extract Sensitive Data

* Downloaded the `webapp.db` SQLite database.
* Queried the database to extract user credentials:

  ```
  SELECT * FROM users;
  ```
* Found the admin user’s password hash:

  ```
  6eea9b7ef19179a06954edd0f6c05ceb
  ```

### ✅ 4. Crack the Hash

* Using **online MD5 hash cracking tools** or **hashcat**, the hash resolved to:

  ```
  qwertyuiop
  ```

### ✅ 5. Login as Admin

* Successfully logged in with:

  ```
  Username: admin
  Password: qwertyuiop
  ```
* Retrieved the final flag:

  ```
  THM{Yzc2YjdkMjE5N2VjMzNhOTE3NjdiMjdl}
  ```

---

## ✅ Key Takeaways

* **Root Cause:** Sensitive files were stored in a publicly accessible directory.

* **Impact:**

  * Disclosure of credentials.
  * Unauthorized admin access.

* **Mitigation:**

  * Avoid storing databases or sensitive files in web-accessible directories.
  * Enforce **least privilege** on file systems and sanitize directory listings.
  * Use **environment variables** or secure storage for credentials.

---

## 🗺 MITRE ATT\&CK Mapping

| **Technique**                           | **ID**                                                      | **Description**                                                                         |
| --------------------------------------- | ----------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| **Exfiltration Over Web Service**       | [T1567.002](https://attack.mitre.org/techniques/T1567/002/) | Adversaries download data from exposed web services or resources.                       |
| **Credentials from Web-Based Database** | [T1552.005](https://attack.mitre.org/techniques/T1552/005/) | Adversaries may obtain credentials stored in databases accessible through a web server. |

---