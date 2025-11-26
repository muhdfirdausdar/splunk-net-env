# Creating Virtual Network Adapter (Windows 10 | 11)

- Create a new virtual network adapter to assign a static IP address for Splunk services.
- The static IP will be used for Splunk Web and other services that need a reachable host address.
- When configuring a Splunk Universal Forwarder on the host, the forwarder needs the indexer IP and port (not `localhost`).
- Splunk Enterprise uses `localhost` (127.0.0.1) by default; using `localhost` as the receiver IP from other machines will not work because it refers only to the local loopback interface.

# Using Device Manager to create the virtual adapter

Below is a short video showing how to create a new virtual adapter. Click the thumbnail to open YouTube.

[![Create Virtual Adapter](https://img.youtube.com/vi/wNzXss3rtXw/hqdefault.jpg)](https://www.youtube.com/watch?v=wNzXss3rtXw)

After creating the adapter, configure it with a static IP. Example values used in this guide:

- IP address: `10.20.30.10`
- Netmask: `255.255.255.0` (prefix `/24`)

Video above also shows steps to add static IP to virtual network adapter



# Setup firewall rules (Host) — allow Splunk ports

What you need:

- The newly created virtual adapter IP (example: `10.20.30.10`).
- The Splunk ports to allow inbound to the host:

```text
Management (Splunkd RPC) -> TCP 8089
Web UI                  -> TCP 8000
Indexer receiver        -> TCP 9997
Syslog (OPNsense)       -> TCP 514
```

Windows GUI steps (Windows Defender Firewall → Advanced Settings):
1. Open `Windows Defender Firewall with Advanced Security`.
2. Click `Inbound Rules` → `New Rule...`.
3. Choose `Port` → Next.
4. Select `TCP`  and `Specific local ports` → enter the port number (e.g. `8089`) → Next.
5. Choose `Allow the connection` → Next.
6. Apply to all profiles (Domain, Private, Public) or choose appropriately → Next.
7. Give the rule a descriptive name (e.g. `Allow Splunk Management 8089`) → Finish.
8. Repeat for each required port.

Other Method: PowerShell example (creates inbound allow rules for TCP ports):

```powershell
New-NetFirewallRule -DisplayName "Allow Splunk Mgmt 8089" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 8089 -Profile Any
New-NetFirewallRule -DisplayName "Allow Splunk Web 8000" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 8000 -Profile Any
New-NetFirewallRule -DisplayName "Allow Splunk Receiver 9997" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 9997 -Profile Any
New-NetFirewallRule -DisplayName "Allow Syslog UDP 514" -Direction Inbound -Action Allow -Protocol UDP -LocalPort 514 -Profile Any
```

If you want to restrict rules to the new adapter only, create the rule and then edit `InterfaceTypes` or use the `-InterfaceAlias` parameter where applicable.

# After completion — verify connectivity

From the host, test the adapter locally:

```powershell
Test-NetConnection -ComputerName 10.20.30.10 -Port 8000
```

From OPNsense (or another VM), test ping and TCP connectivity (adjust for your environment):

CMD (ping):

```cmd
ping 10.20.30.10
```

If you see successful replies and open ports, the adapter and firewall rules are working.

---




