# Linux for SOC Analysts

A practical, security-focused guide to using Linux for log analysis, incident investigation, and basic threat detection.
This repository is designed for beginner SOC analysts who want to build hands-on Linux investigation skills.
## Why Linux is Important in SOC

Many production servers run Linux. SOC analysts frequently analyze:

- SSH authentication logs
- sudo activity
- File permission misconfigurations
- Suspicious processes
- Network connections

Understanding Linux at investigation level is essential for detecting privilege escalation, brute-force attacks, and persistence techniques.
## Important Directories for Security Monitoring

### /etc/passwd
Contains user account information.

Security relevance:
- Detect new user creation
- Check login shell modifications

### /etc/shadow
Stores password hashes.

Security relevance:
- Must not be world-readable
- Exposure = Confidentiality violation

### /etc/group
Stores group memberships.

Security relevance:
- Detect privilege escalation (sudo group changes)

### /var/log
Contains system and authentication logs.

Security relevance:
- Primary source of investigation data
## File Permissions (rwx)

Permission structure:
Owner | Group | Others

Numeric values:
r = 4
w = 2
x = 1

Example:
-rwxr-xr-x = 755

### Security Risk: World-Writable Files (777)

If a root-owned file has 777 permissions:
- Any user can modify it
- If executed by root, attacker can escalate privileges
- This violates system integrity
  
