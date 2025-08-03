# 📉 Insufficient Logging and Monitoring – TryHackMe Walkthrough

## 📝 Overview

**Insufficient Logging and Monitoring** is an often-overlooked vulnerability that significantly increases the impact of attacks. When web applications fail to log key user actions or do not actively monitor for suspicious behavior, it becomes extremely difficult to detect and respond to breaches in a timely manner.

Attackers often exploit this lack of visibility to carry out prolonged attacks undetected, such as brute-force login attempts, data exfiltration, or privilege escalation.

---

## 🎯 Lab Objective

* Analyze a sample web server log file.
* Identify attacker behavior based on logged data.
* Determine the attack type and the IP address responsible.

---

## 🛠 Steps to Solve

### ✅ 1. Log Analysis

* Loaded the sample log file provided in the challenge.
* Reviewed fields including:

  * **IP addresses**
  * **Timestamps**
  * **Requested endpoints**
  * **HTTP status codes**
  * **Usernames attempted**

---

### ✅ 2. Identifying the Attacker’s IP

* Detected a high number of repeated login attempts from a single IP address:

  ```
  49.99.13.16
  ```

* **Flag Retrieved:**

  ```
  49.99.13.16
  ```

---

### ✅ 3. Determining the Attack Type

* The attacker was repeatedly attempting to log in with different usernames and passwords.

* This pattern of requests indicates a classic **Brute Force** attack.

* **Flag Retrieved:**

  ```
  Brute Force
  ```

---

## ✅ Key Takeaways

* **Root Cause:** Logs were not being actively monitored, allowing repeated malicious activity to go undetected.
* **Impact:**

  * Delayed detection and response to ongoing attacks
  * Increased risk of unauthorized access and data theft
* **Mitigation:**

  * Log critical actions like failed logins, admin access attempts, and suspicious payloads
  * Monitor logs in real-time and implement alerting systems
  * Store logs securely and replicate to avoid data loss
  * Use tools like SIEM (Security Information and Event Management) to detect anomalies

---

## 🗺 MITRE ATT\&CK Mapping

| **Technique**                    | **ID**                                              | **Description**                                                  |
| -------------------------------- | --------------------------------------------------- | ---------------------------------------------------------------- |
| **Brute Force – Valid Accounts** | [T1110](https://attack.mitre.org/techniques/T1110/) | Repeated password guessing attempts to gain unauthorized access. |
| **Impair Defenses (Logging)**    | [T1562](https://attack.mitre.org/techniques/T1562/) | Exploiting lack of logging and alerting to maintain stealth.     |
