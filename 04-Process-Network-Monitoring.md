## Day 5 – Process Investigation

### Scenario

An unknown process was suspected to be running on the system.
The objective was to identify whether the process was legitimate or malicious.

---

### Commands Used

ps aux
top
htop
pstree -p

---

### Investigation Steps

1. Listed all running processes using `ps aux`
2. Monitored real-time process activity using `top`
3. Analyzed process hierarchy using `pstree`
4. Identified suspicious processes and behavior

---

### Findings

* Reviewed running processes and their resource usage
* Checked for unknown or suspicious process names
* Analyzed parent-child relationships of processes

---

### SOC Analysis

Indicators of suspicious processes:

* unknown process names
* high CPU or memory usage
* unusual execution paths
* suspicious parent-child process relationships

---

### Severity Assessment

| Scenario                       | Severity |
| ------------------------------ | -------- |
| Normal processes               | Low      |
| Unknown process detected       | Medium   |
| High resource usage            | High     |
| Reverse shell pattern detected | Critical |

---

### Conclusion

Process investigation is essential for detecting malware and unauthorized activity.
Monitoring running processes and their relationships helps identify potential threats on Linux systems.

