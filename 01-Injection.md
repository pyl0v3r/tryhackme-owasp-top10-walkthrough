Got it! Let me draft a detailed **Injection walkthrough** for your GitHub page based on what you learned and the questions you answered.

---

# 🔥 OWASP Top 10 – Injection

## ✅ Summary

Injection vulnerabilities occur when untrusted input is executed as code or commands by the target system, leading to command execution or data leakage. In this lab, the focus was on **OS Command Injection** via a vulnerable PHP web page that allowed inputting Linux commands.

---

## 🛠 Lab Walkthrough: OS Command Injection

### Lab Setup

* A PHP page with an input field accepting commands.
* Input was executed on the server shell, enabling arbitrary Linux command execution.

### Tasks and Solutions

1. **Find a strange file in the home directory**

```bash
ls /home
ls /home/strangefile
# or just ls /home | grep strange
```

*Used `ls /home` to discover a strange or unusual file present.*

2. **Find non-root, non-daemon, non-service users**

```bash
cat /etc/passwd | grep -v 'root\|daemon\|service'
```

*Filtered users excluding root, daemon, and service — result was empty, confirming no such users.*

3. **What user is this app running as?**

```bash
whoami
```

*Discovered the current user under which the PHP app runs.*

4. **What is the user’s shell set as?**

```bash
echo $SHELL
# or check /etc/passwd entry for current user:
grep $(whoami) /etc/passwd | cut -d: -f7
```

*Identified the default shell of the app user.*

5. **What version of Ubuntu is running?**

```bash
cat /etc/os-release
# or
lsb_release -a
```

*Read the OS release file to find Ubuntu version.*

6. **Print out the MOTD. What favorite beverage is shown?**

```bash
cat /etc/motd
# or check /etc/update-motd.d/00-header if motd is empty
cat /etc/update-motd.d/00-header
```

*Displayed the Message of the Day (MOTD) — the output included a favorite beverage.*

---

## 🔗 MITRE ATT\&CK Mapping

| MITRE Technique                   | ID                                                  | Description                                                                              |
| --------------------------------- | --------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| Command-Line Interface            | [T1059](https://attack.mitre.org/techniques/T1059/) | Adversaries abuse command-line interfaces for execution. OS command injection fits here. |
| Exploit Public-Facing Application | [T1190](https://attack.mitre.org/techniques/T1190/) | Exploiting vulnerable web apps, e.g., via injection, to run commands or escalate access. |

---

## 🧠 Key Takeaways

* Unsanitized user inputs in web apps can lead to arbitrary OS command execution.
* Always validate and sanitize inputs and avoid directly passing user input to shell commands.
* Using least privileged users for web services limits damage in case of exploitation.
* Understanding the environment (user, OS version, etc.) helps attackers enumerate targets — defensive logging and monitoring can detect this activity.

---