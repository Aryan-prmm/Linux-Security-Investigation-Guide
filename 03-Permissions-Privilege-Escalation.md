## Day 3 – User Account Investigation

### Scenario

A potential unauthorized user account was suspected on the system.
The objective was to identify unusual or privileged user accounts.

---

### Commands Used

cat /etc/passwd
cat /etc/shadow
id <username>
groups <username>

---

### Investigation Steps

1. Listed all users using `/etc/passwd`
2. Checked password and account status using `/etc/shadow`
3. Verified user privileges using `id`
4. Checked group memberships using `groups`

---

### Findings

* Reviewed all system users
* Checked for unknown or suspicious accounts
* Verified privilege levels of users

---

### UID 0 Check

Command used:

```
awk -F: '$3 == 0 {print $1}' /etc/passwd
```

Only the root user was expected to have UID 0.

---

### SOC Analysis

Indicators of compromise:

* unknown user accounts
* users with sudo privileges
* multiple UID 0 accounts

---

### Severity Assessment

| Scenario              | Severity |
| --------------------- | -------- |
| Normal users          | Low      |
| Unknown user detected | Medium   |
| User with sudo access | High     |
| Multiple UID 0 users  | Critical |

---

### Conclusion

User account investigation is critical for detecting unauthorized access and persistence mechanisms.
Monitoring user accounts and privilege levels helps identify potential security threats.

