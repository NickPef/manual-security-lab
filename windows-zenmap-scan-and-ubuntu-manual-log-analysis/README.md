# windows-zenmap-initiated-scan-and-ubuntu-manual-log-detection

## Scenario
A controlled network scan was launched from a Windows machine using Zenmap against an Ubuntu host to simulate reconnaissance activity during the initial phase of a potential cyber attack.

The purpose of this lab was to observe how network scanning behavior can be detected using firewall logs and real-time traffic monitoring.

## Environment

### Target System
- Ubuntu Desktop  
- UFW Firewall  
- systemd journal logging system  
- Kernel log monitoring tools  

### Attacker Simulation System
- Windows Laptop  
- Zenmap (Nmap GUI)

## Objective
- Generate detectable scan traffic  
- Monitor firewall logs in real time  
- Identify blocked connection attempts  
- Understand how reconnaissance appears in Linux system logs  

---

## Zenmap Scan Commands (Windows Machine)

### Intense Reconnaissance Scan

```bash
**nmap -T4 -A -v <TARGET_IP>**
-- Command Explanation

-T4 → Sets scan timing to aggressive mode, increasing scanning speed and traffic intensity.

-A → Enables advanced detection features including:

OS fingerprinting

Service version detection

Script scanning

Traceroute analysis

-v → Enables verbose output to display detailed scan progress.

**This scan type generates significant network activity and is useful for detection testing in controlled lab environments**

## Uuntu Detection Commands
Rotate and Clean Old Logs
sudo journalctl --rotate
sudo journalctl --vacuum-time=1s
Explanation

Forces log rotation to create a clean monitoring environment.

Removes archived logs older than 1 second for testing purposes.

Configure Firewall Logging
sudo ufw default deny incoming
sudo ufw logging medium
sudo ufw reload
Explanation

default deny incoming → Blocks unsolicited inbound connections.

logging medium → Enables balanced firewall event logging.

reload → Applies firewall configuration changes.

Real-Time Log Monitoring
sudo journalctl -kf | grep UFW
Explanation

-k → Displays kernel messages where firewall events are recorded.

-f → Follows logs live in real time.

grep UFW → Filters firewall-related entries.

Monitor Blocked Connection Attempts Only
sudo journalctl -k -f | grep BLOCK
Explanation

Displays only dropped packets.

Useful for identifying reconnaissance scanning behavior.

Keep this terminal window open during scan execution.

Log Investigation
View Logs From Current Boot Session
sudo journalctl -k --since today
Search Logs by Attacker IP
sudo journalctl -k | grep <ATTACKER_IP>
Conclusion
Skills Demonstrated

Host-based firewall configuration

Threat detection fundamentals

Log analysis and monitoring

Network security awareness

Blue team defensive validation

Key Takeaways

Default-deny firewall policies improve security posture.

Real-time log monitoring enables rapid reconnaissance detection.

Even basic scanning attempts can be identified in system telemetry.

Kernel-level logs provide valuable security visibility.

Author

Personal Security Lab

Focus

Offensive simulation and defensive detection validation.
