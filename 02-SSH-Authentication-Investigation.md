# Day 1-SSH Brute Force Detection Investigation (Kali Linux)

## Scenario

Multiple failed SSH login attempts may indicate a brute force attack targeting a Linux server.
This investigation demonstrates how to detect such activity using system logs.

---

# Environment

Operating System: Kali Linux
SSH Service: OpenSSH
Log Source: systemd journal (journalctl)

---

# Problem Encountered During Investigation

Initially, the authentication log was expected at:

```
/var/log/auth.log
```

However, the file did not exist.

Command used:

```
ls /var/log/auth.log
```

Output:

```
No such file or directory
```

### Reason

Some Linux distributions (including certain Kali setups) store authentication logs in **systemd journal** instead of `/var/log/auth.log`.

---

# Solution: Using journalctl for SSH Logs

To view SSH logs:

```
journalctl -u ssh
```

This command shows logs related to the SSH service.

Example output:

```
sshd[4310]: Server listening on 0.0.0.0 port 22
sshd-session[6776]: Accepted password for kali from ::1 port 34394 ssh2
```

---

# Issue 2 – No Failed Login Entries

When running:

```
journalctl -u ssh | grep "Failed password"
```

No output was returned.

### Reason

No failed login attempts had occurred yet.

---

# Generating Failed Login Attempts (Lab Simulation)

To simulate a brute force attempt, a login was attempted with an invalid user.

Command:

```
ssh fakeuser@localhost
```

A wrong password was entered multiple times.

This generated authentication failure logs.

---

# Detecting Failed SSH Login Attempts

Command used:

```
journalctl -u ssh | grep "Failed password"
```

Example log entry:

```
Failed password for invalid user fakeuser from ::1 port 50211 ssh2
```

Information obtained from this log:

* attempted username
* source IP address
* authentication method
* port used

---

# Extracting Attacker IP Address

Command:

```
journalctl -u ssh | grep "Failed password" | awk '{print $(NF-3)}'
```

Example output:

```
::1
::1
::1
```

Explanation:

`::1` represents the localhost IPv6 address.

---

# Counting Login Attempts Per IP

Command:

```
journalctl -u ssh | grep "Failed password" | awk '{print $(NF-3)}' | sort | uniq -c
```

Example output:

```
7 ::1
```

Interpretation:

Multiple failed attempts from a single IP may indicate a brute force attack.

---

# Checking if Authentication Was Successful

Command:

```
journalctl -u ssh | grep "Accepted password"
```

Example output:

```
Accepted password for kali from ::1 port 34394 ssh2
```

This indicates a successful login for the legitimate user.

No successful login was observed for the attacker.

---

# Investigation Findings

Indicators observed during the investigation:

* repeated failed SSH login attempts
* authentication failures targeting an invalid user
* attempts originating from a single IP address

These indicators are commonly associated with brute force attacks.

---

# SOC Risk Assessment

| Security Aspect | Impact                                       |
| --------------- | -------------------------------------------- |
| Confidentiality | attacker could gain access to sensitive data |
| Integrity       | attacker could modify system files           |
| Availability    | attacker could disrupt services              |

---

# Severity Assessment

| Scenario                     | Severity |
| ---------------------------- | -------- |
| Few failed attempts          | Low      |
| Multiple failed attempts     | Medium   |
| Successful brute force login | High     |
| Root account compromise      | Critical |

For this investigation:

Severity Level: **Medium**

Reason: multiple authentication failures but no successful compromise.

---

# Conclusion

SSH authentication logs can be analyzed to detect brute force attacks.
Repeated failed login attempts from the same IP address are a strong indicator of such activity.

When `/var/log/auth.log` is not available, SSH activity can be investigated using `journalctl`.

Monitoring authentication logs is a critical task for SOC analysts to identify and respond to unauthorized access attempts.

