# DVWA Network Troubleshooting Lab

## Scenario
- DVWA was hosted on an Ubuntu desktop but was not accessible from a Windows laptop on the same network.
- Goal Identify the root cause and restore connectivity.

## Environment
**Target:** Ubuntu Desktop  
- Apache2  
- DVWA  
- Local network IP  
- Client: Windows Laptop
  
## Problem
DVWA opened locally on Ubuntu but could not be reached from Windows via:

<ubuntu-ip>/dvwa

Connectivity tests were failing.

## Troubleshooting Process

### 1) Apache Verification
Checked if Apache was running: 

**sudo** systemctl status apache2

### 2) Port Check
Verified that port 80 was listening:

**sudo** ss -tulnp | grep :80

 Result:
LISTEN 0 511 *:80
Apache was accepting connections.

### 3) Network Connectivity Test
From Windows:

Test-NetConnection <ubuntu-ip> -Port 80

Initial result:
TcpTestSucceeded : False

Meaning the service was unreachable despite being active.

### 4) Firewall Investigation

Checked firewall configuration on Ubuntu.

Identified that the connection zone was not properly configured.

used NetworkManager to inspect the interface:

**nmcli connection show**

Located active interface:

netplan-enpxxx

Checked zone:

**nmcli connection show netplan-enpxxx | grep zone**

### 5)Fix Applied

Assigned the connection to a trusted zone and reloaded the connection

After adjustment:

**Test-NetConnection <ubuntu-ip> -Port 80**

Result:

TcpTestSucceeded : True

✅ investigation  
✅ process  
✅ outcome  
### DVWA became accessible from the Windows machine.



















