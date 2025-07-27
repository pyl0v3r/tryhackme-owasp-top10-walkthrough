# 🔐 Broken Authentication – TryHackMe Walkthrough

## 📝 Overview

This lab demonstrates a **Broken Authentication** vulnerability caused by **improper input validation** during user registration. By manipulating the input field (adding a space before the username), we can re-register an existing user and gain unauthorized access to their account.

---

## 🎯 Lab Objective

* **Exploit a re-registration flaw** to gain access to another user’s account.
* Retrieve secret flags from compromised user accounts.

---

## 🛠 Steps to Exploit

1. **Attempt to register an existing user (`darren`)**

   * The application rejects it, showing that the username already exists.

2. **Re-register the same username with a space prefix**

   * Input: `␣darren` (`" darren"`, with a space at the start).
   * Submit with any email/password.

3. **Successful login as Darren**

   * The system treats this new account as the original `darren`, granting access to Darren’s private data.
   * **Flag retrieved:**

     ```
     fe86079416a21a3c99937fea8874b667
     ```

4. **Repeat the same process for `arthur`**

   * Input: `␣arthur` (`" arthur"`, with a space at the start).
   * Gain access to Arthur’s account.
   * **Flag retrieved:**
   
     ```
     d9ac0f7db4fda460ac3edeb75d75e16e
     ```

---

## ✅ Key Takeaways

* **Root Cause:**

  * The application does not trim or sanitize input, allowing users to bypass unique username checks.

* **Impact:**

  * Unauthorized account access.
  * Potential privilege escalation if the compromised account has admin rights.

* **Mitigation:**

  * Enforce strict input validation and trimming of whitespace.
  * Implement **unique constraints** at the database level.

---

## 🗺 MITRE ATT\&CK Mapping

| **Technique**            | **ID**                                              | **Description**                                                    |
| ------------------------ | --------------------------------------------------- | ------------------------------------------------------------------ |
| **Valid Accounts**       | [T1078](https://attack.mitre.org/techniques/T1078/) | Adversaries obtain and abuse legitimate user credentials/accounts. |
| **Account Manipulation** | [T1098](https://attack.mitre.org/techniques/T1098/) | Adversaries may create or modify accounts to maintain access.      |

---