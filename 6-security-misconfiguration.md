# ⚙️ Security Misconfiguration (Default Credentials) – TryHackMe Walkthrough

## 📝 Overview

This challenge demonstrates **Security Misconfiguration**, one of the **OWASP Top 10** vulnerabilities. Security misconfigurations occur when applications, services, or infrastructure are not securely configured—such as leaving default credentials unchanged, enabling unnecessary services, or revealing sensitive system information.

In this case, the web application was deployed using default settings, allowing access via **default credentials publicly available online**.

---

## 🎯 Lab Objective

* Identify a **security misconfiguration** in a hosted web application.
* Exploit **default credentials** to gain unauthorized access.
* Retrieve the flag from within the application.

---

## 🛠 Steps to Exploit

### ✅ 1. Understanding the Vulnerability

* Discovered the web application **Pensive Notes** running on the target machine.
* Security misconfigurations often involve:

  * Default credentials
  * Unnecessary enabled services
  * Detailed error messages or exposed documentation

---

### ✅ 2. Investigating Public Information

* Searched **“Pensive Notes GitHub”** on Google to locate its official repository.
* Found installation documentation, which listed the **default credentials**:

  ```
  Username: pensive
  Password: PensiveNotes
  ```

---

### ✅ 3. Exploiting the Default Credentials

* Logged into the hosted **Pensive Notes** application using the default credentials.

* Successfully accessed the admin dashboard and retrieved the flag.

* **Flag Retrieved:**

  ```
  thm{4b9513968fd564a87b28aa1f9d672e17}
  ```

---

## ✅ Key Takeaways

* **Root Cause:** Default credentials were not changed after deployment, allowing unauthorized access.
* **Impact:**

  * Unauthorized administrative access
  * Potential for data exposure or further compromise
* **Mitigation:**

  * Change all default credentials before deploying applications.
  * Regularly audit for misconfigurations.
  * Follow **least privilege** principles and disable unnecessary features.

---

## 🗺 MITRE ATT\&CK Mapping

| **Technique**                            | **ID**                                              | **Description**                                                    |
| ---------------------------------------- | --------------------------------------------------- | ------------------------------------------------------------------ |
| **Exploitation of Public-Facing App**    | [T1190](https://attack.mitre.org/techniques/T1190/) | Exploiting a web app misconfiguration to gain unauthorized access. |
| **Valid Accounts (Default Credentials)** | [T1078](https://attack.mitre.org/techniques/T1078/) | Using default credentials to gain administrative access.           |
