# VirtualBox | Host | OPNsense Setup

What to do:
- Create 1x Host-Only Network with IP 192.168.10.1/24
- Modify Host VM Network to Host-only Adapter
- Modify OPNsense VM Network to Link Host VM

Click File > Tools > Network
Under Host-only Networks
    Click Create
    Wait until new Host-Only Ethernet Adapter completely created
    Click or Choose the Adapter
    Do Configure Adapter Manually
    Add IPv4 Adddress: 192.168.10.1
    Network Mask: 255.255.255.0
    Remember the network adapter name
    Click Apply and Return Back

Choose the Host VM (Mine: Win10 Lab PC1)
Go to Settings
    Go to Network
    At Adapter 1, change Attached to Host-only Adapter
    Change Name to Host-Only Ethernet Adapter you have created earlier
    Click OK and Return Back

Choose the OPNsense Lab
Go to Settings
    Go to Network
    At Adapter 1, make sure Attached to : NAT
    Click Adapter 2, Enable Network Adapter
    At Adapter 2, change Attached to Host-only Adapter
    Change Name to Host-Only Ethernet Adapter you have created earlier (same as Host VM)
    Click OK and Return Back

Next we need to start both VM to configure the network 

## Setup Host Windows 10

Below is the YouTube link showing how to configure your network adapter. Click the thumbnail to open the video.

[![Configure Network Adapter](https://img.youtube.com/vi/63-iVcseSps/hqdefault.jpg)](https://www.youtube.com/watch?v=63-iVcseSps)

My Host uses IPv4:

- Host IP: `192.168.10.10`
- Gateway: `192.168.10.254`
- Mask: `255.255.255.0`
- DNS: `1.1.1.1` and `8.8.8.8`


Please make sure Host VM Firewall already turned off to ensure connection between Host and OPNsense.

# Setup OPNsense

Follow the YouTube video showing how to configure OPNsense in VirtualBox. Click the thumbnail to open the video.

[![Configure OPNsense in VirtualBox](https://img.youtube.com/vi/naYU8UR5BhQ/hqdefault.jpg)](https://www.youtube.com/watch?v=naYU8UR5BhQ)

- My WAN or NAT Adapter is called `em0` in OPNsense (Adapter 1 in VirtualBox settings).
- `em1` is Adapter 2 and connects the OPNsense LAN to the Host VM.
- When configuring `em1`, set the LAN gateway IP to match the host gateway (example: `192.168.10.254`) and use the same subnet mask as the host.

## After completion

You should able to ping Host IP address from OPNsense terminal and vice versa

