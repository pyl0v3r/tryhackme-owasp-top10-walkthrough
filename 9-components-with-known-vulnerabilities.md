# 🧩 Components with Known Vulnerabilities – TryHackMe Walkthrough

## 📝 Overview

**Components with Known Vulnerabilities** refers to the use of libraries, frameworks, or software modules that contain publicly known security flaws. Attackers can exploit these vulnerabilities when software isn't updated or patched regularly.

This challenge demonstrates how an attacker can leverage a **publicly known exploit** against a vulnerable application component to gain command execution.

---

## 🎯 Lab Objective

* Identify a component in the web app that uses a vulnerable version.
* Research a public exploit online.
* Use the exploit to execute a command on the server.
* Retrieve system information using Linux commands.

---

## 🛠 Steps to Exploit

### ✅ 1. Reconnaissance

* Visited the web application and identified the software and version number in use.
* Performed a quick online search (e.g., `vulnerable-software-name version exploit site:exploit-db.com`) to locate a **public exploit script**.

---

### ✅ 2. Finding and Running the Exploit

* Located a known Remote Code Execution (RCE) exploit online for the vulnerable component.

* Modified and executed the script with a payload wrapped in **double quotes**, as per lab instructions.

  **Command sent via the exploit:**

  ```
  "wc -c /etc/passwd"
  ```

* This command counts the number of characters in the `/etc/passwd` file.

---

### ✅ 3. Result

* The output returned:

  ```
  1611
  ```

* **Flag Retrieved / Correct Answer:**

  ```
  1611
  ```

---

## ✅ Key Takeaways

* **Root Cause:** Application used outdated components with known public exploits.
* **Impact:**

  * Remote Code Execution
  * Unauthorized access to system files
* **Mitigation:**

  * Regularly update software dependencies and third-party components
  * Subscribe to vulnerability databases (e.g., CVE, NVD, GitHub Advisories)
  * Use tools like **OWASP Dependency-Check**, **Snyk**, or **Retire.js** for automated scanning

---

## 🗺 MITRE ATT\&CK Mapping

| **Technique**                            | **ID**                                              | **Description**                                                     |
| ---------------------------------------- | --------------------------------------------------- | ------------------------------------------------------------------- |
| **Exploitation of Vulnerable Component** | [T1190](https://attack.mitre.org/techniques/T1190/) | Public exploit used to trigger code execution on outdated software. |
| **Command and Scripting Interpreter**    | [T1059](https://attack.mitre.org/techniques/T1059/) | Executed Linux command through remote access.                       |
