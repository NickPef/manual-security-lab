# Windows-zenmap-initiated-scan-and-Ubuntu-manual-log-detection

## Scenario
A controlled network scan was launched from a Windows machine using Zenmap against an Ubuntu host to simulate reconnaissance activity during the initial phase of a potential cyber attack.

The purpose of this lab was to observe how network scanning behavior can be detected using firewall logs and real-time traffic monitoring.

---

## Environment

### Target System
- Ubuntu Desktop  
- UFW Firewall  
- systemd journal logging system  
- Kernel log monitoring tools  

### Attacker Simulation System
- Windows Laptop  
- Zenmap (Nmap GUI)

---

## Objective

- Generate detectable scan traffic  
- Monitor firewall logs in real time  
- Identify blocked connection attempts  
- Understand how reconnaissance appears in Linux system logs  

---

## Zenmap Scan Commands (Windows Machine)

### Intense Reconnaissance Scan

bash
nmap -T4 -A -v <TARGET_IP>

**Command Explanation**

-T4 → Aggressive scan timing for faster and more noisy reconnaissance traffic.

-A → Enables advanced detection features including:

- OS fingerprinting

- Service version detection

- Script scanning

- Traceroute analysis

-v → Displays verbose output for detailed scan monitoring.

**This scan type generates significant network activity and is useful for detection testing in controlled lab environments***

## Ubuntu Detection Commands

**Rotate and Clean Old Logs**

1) sudo journalctl --rotate
- Forces systemd to archive current logs and start a new logging cycle.

2) sudo journalctl --vacuum-time=1s
- Removes archived logs older than 1 second to ensure a clean testing environment.

**Configure Firewall Logging**

3) sudo ufw default deny incoming
- Blocks all unsolicited incoming network connections by default.

4)sudo ufw logging medium
- Enables balanced firewall logging verbosity.

5) sudo ufw reload
Applies firewall configuration changes.

**Real-Time Log Monitoring**

6) sudo journalctl -kf | grep UFW

- -k → Displays kernel messages where firewall events are recorded.

- -f → Follows logs live in real time.

- grep UFW → Filters firewall-related firewall entries.

Keep this terminal open during scan execution.

**Monitor Blocked Connection Attempts Only**

7) sudo journalctl -k -f | grep BLOCK

Displays only dropped packets, which is useful for detecting reconnaissance scanning activity.

**Log Investigation**

**View Logs From Current Boot Session**

8) sudo journalctl -k --since today

Displays kernel logs generated during the current system boot session.

**Search Logs by Attacker IP**

9) sudo journalctl -k | grep <ATTACKER_IP>

Filters logs associated with a specific scanning source IP address.

## Conclusion
**Skills Demonstrated**

- Host-based firewall configuration
- Threat detection fundamentals
- Network log analysis and monitoring
- Security awareness of reconnaissance behavior
- Blue team defensive validation

**Key Takeaways**

- Default-deny firewall policies improve security posture.
- Real-time log monitoring enables rapid reconnaissance detection.
- Even basic scanning attempts can be identified in system telemetry.
- Kernel-level logs provide valuable security visibility



**Personal Security Lab**
**Offensive simulation and defensive detection validation**

