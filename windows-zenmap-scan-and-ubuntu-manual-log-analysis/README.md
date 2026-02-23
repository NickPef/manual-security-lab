Command Explanation

-T4 → Aggressive scan timing for faster and more noisy reconnaissance traffic.

-A → Enables advanced detection features including:

OS fingerprinting

Service version detection

Script scanning

Traceroute analysis

-v → Displays verbose output for detailed scan monitoring.

This scan type generates significant network activity and is useful for detection testing in controlled lab environments.

Ubuntu Detection Commands
Rotate and Clean Old Logs
sudo journalctl --rotate

Forces systemd to archive current logs and start a new logging cycle.

sudo journalctl --vacuum-time=1s

Removes archived logs older than 1 second to ensure a clean testing environment.

Configure Firewall Logging
sudo ufw default deny incoming

Blocks all unsolicited incoming network connections by default.

sudo ufw logging medium

Enables balanced firewall logging verbosity.

sudo ufw reload

Applies firewall configuration changes.

Real-Time Log Monitoring
sudo journalctl -kf | grep UFW

-k → Displays kernel messages where firewall events are recorded.

-f → Follows logs live in real time.

grep UFW → Filters firewall-related firewall entries.

Keep this terminal open during scan execution.

Monitor Blocked Connection Attempts Only
sudo journalctl -k -f | grep BLOCK

Displays only dropped packets, which is useful for detecting reconnaissance scanning activity.

Log Investigation
View Logs From Current Boot Session
sudo journalctl -k --since today

Displays kernel logs generated during the current system boot session.

Search Logs by Attacker IP
sudo journalctl -k | grep <ATTACKER_IP>

Filters logs associated with a specific scanning source IP address.

Conclusion
Skills Demonstrated

Host-based firewall configuration

Threat detection fundamentals

Network log analysis and monitoring

Security awareness of reconnaissance behavior

Blue team defensive validation

Key Takeaways

Default-deny firewall policies improve security posture.

Real-time log monitoring enables rapid reconnaissance detection.

Even basic scanning attempts can be identified in system telemetry.

Kernel-level logs provide valuable security visibility.

Author

Personal Security Lab

Focus

Offensive simulation and defensive detection validation

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

---

This scan type generates significant network activity and is useful for detection testing in controlled lab environments

## Uuntu Detection Commands

- Rotate and Clean Old Logs
- sudo journalctl --rotate
- sudo journalctl --vacuum-time=1s
- Explanation

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
