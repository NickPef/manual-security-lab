# DVWA Network Troubleshooting Lab

## Scenario
The DVWA was hosted on an Ubuntu Desktop system running Apache2.
The application was accessible locally from the Ubuntu machine but could not be accessed remotely from a Windows laptop on the same LAN using

- http://Ubuntu-IP/DVWA

Initial connectivity http://<Ubuntu-IP>/DVWA testing from Windows PowerShell indicated that port 80 was unreachable

## Goal
Identify the root cause and restore connectivity.

## Environment
- Target: Ubuntu Desktop
  - Apache2
  - DVWA
  - Local network IP
- Client: Windows Laptop

## Problem
DVWA opened locally on Ubuntu but could not be reached from Windows via `http://<Ubuntu-IP>/DVWA. Connectivity tests were failing.

## Troubleshooting Process

1. **Apache Verification**
     
  Command
  
- sudo systemctl status apache2

**What Was Verified**

- Apache service was active (running).
- Worker processes were operational.
- No service-level errors detected.

- Conclusion
- The web server itself was functioning correctly.
  
   2. **Port Check**
  
  command

- sudo ss -tulnp | grep :80

**What Was Verified**

Port 80 was in LISTEN state.
Apache was bound to the correct TCP port.
Conclusion
The service was properly listening for incoming HTTP connections.

3. **Network Connectivity Test(Client Side)**

Command (Windows PowerShell)
Test-NetConnection -ComputerName <Ubuntu-IP> -Port 80
Result (Initial State)
TcpTestSucceeded : False

**Interpretation**

The Windows client could not establish a TCP connection to port 80.
This indicated a network-level restriction, not an application failure

**Initial Result: TcpTestSucceeded : False → Service unreachable**

4. **Firewall / Network Zone Investigation**

Checked firewall configuration on Ubuntu. Found that the connection zone was not properly configured.

nmcli connection show

nmcli connection show netplan-enpxxx | grep zone

What Was Discovered
The active network interface was not assigned to a trusted zone.
Firewall policy was restricting inbound traffic.

Diagnosis
The issue was caused by firewall zone configuration blocking external access, despite Apache functioning normally.


5. **Fix Applied**
The active network connection was assigned to a trusted firewall zone and the configuration was reloaded.

After applying the changes, network policies allowed inbound traffic on port 80.

✅ Verification
Connectivity was tested again from the Windows machine.

Command

Test-NetConnection -ComputerName <Ubuntu-IP> -Port 80

Result

TcpTestSucceeded : True

**Confirmation**

Successful TCP connection to port 80.
DVWA accessible remotely via browser.

🎯 Root Cause

The issue was not related to Apache or DVWA configuration.
It was caused by incorrect firewall zone assignment on the Ubuntu host, which blocked inbound network traffic from the local LAN.

🧾 Final Outcome

After correcting the firewall zone configuration and validating connectivity:
Apache service confirmed operational
Port 80 verified open
Firewall policy corrected
Remote access to DVWA successfully restored
















   

