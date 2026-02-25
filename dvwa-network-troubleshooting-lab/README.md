# DVWA Network Troubleshooting Lab

## Scenario
The DVWA was hosted on an Ubuntu Desktop system running Apache2.
The application was accessible locally but not remotely from a Windows laptop on the same LAN using:

http://Ubuntu-IP/DVWA 

---

## Goal
Identify the root cause and restore connectivity.

---

## Environment

### Target
- Ubuntu Desktop
- Apache2
- DVWA
- Local Network IP

### Client
- Windows Laptop

---

# Troubleshooting Process

---

## 1️⃣ Apache Verification

### Command
```bash
sudo systemctl status apache2
```

### What Was Verified
- Apache service was active (running)
- Worker processes were operational
- No service-level errors detected



### Conclusion
The web server itself was functioning correctly.

---

## 2️⃣ Port Check

### Command
```bash
sudo ss -tulnp | grep :80
```

### What Was Verified
- Port 80 was in LISTEN state
- Apache was bound to the correct TCP port



![Port 80 Listening](troubleshooting/02_port_listen.png)

### Conclusion
The service was properly listening for incoming HTTP connections.

---

## 3️⃣ Network Connectivity Test (Client Side)

### Command (Windows PowerShell)
```powershell
Test-NetConnection -ComputerName <Ubuntu-IP> -Port 80
```

### Initial Result
```
TcpTestSucceeded : False
```



### Interpretation
The Windows client could not establish a TCP connection.
This indicated a network-level restriction.

---

## 4️⃣ Firewall / Network Zone Investigation

### Commands
```bash
nmcli connection show
```

```bash
nmcli connection show netplan-enpxxx | grep zone
```

### What Was Discovered
- The active network interface was not assigned to a trusted zone
- Firewall policy was restricting inbound traffic

📸 Screenshot:


### Diagnosis
Firewall zone configuration was blocking external access.

---

# Fix Applied

The active network connection was assigned to a trusted firewall zone and reloaded.

---

# Verification

### Command
```powershell
Test-NetConnection -ComputerName <Ubuntu-IP> -Port 80
```

### Result
```
TcpTestSucceeded : True
```


### Confirmation
- Successful TCP connection
- DVWA accessible remotely

---

# Root Cause

Incorrect firewall zone assignment on the Ubuntu host blocked inbound LAN traffic.

---

# Final Outcome

- Apache confirmed operational
- Port 80 verified open
- Firewall policy corrected
- Remote access restored
