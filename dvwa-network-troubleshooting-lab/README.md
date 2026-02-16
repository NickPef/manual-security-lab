# DVWA Network Troubleshooting Lab

## Scenario
DVWA was hosted on an Ubuntu desktop but was **not accessible** from a Windows laptop on the same network.

## Goal
Identify the root cause and restore connectivity.

## Environment
- Target: Ubuntu Desktop
  - Apache2
  - DVWA
  - Local network IP
- Client: Windows Laptop

## Problem
DVWA opened locally on Ubuntu but could not be reached from Windows via `http://<Ubuntu-IP>/dvwa`. Connectivity tests were failing.

## Troubleshooting Process

1. **Apache Verification**  
   Checked if Apache was running:  
   ```bash
   sudo systemctl status apache2

2. **Port Check**
Verified that port 80 was listening:

sudo ss -tulnp | grep :80

**Result: LISTEN → Apache was accepting connections.**

3. **Network Connectivity Test**
From Windows:

Test-NetConnection -Port 80

**Initial Result: TcpTestSucceeded : False → Service unreachable**

4. **Firewall Investigation**
Checked firewall configuration on Ubuntu. Found that the connection zone was not properly configured.

nmcli connection show
nmcli connection show netplan-enpxxx | grep zone


5. **Fix Applied**
Assigned the connection to a trusted zone and reloaded the connection.

Test-NetConnection -Port 80

 **Result: TcpTestSucceeded : True**


**Outcome**
✅ **DVWA became accessible from the Windows machine.**




















   

