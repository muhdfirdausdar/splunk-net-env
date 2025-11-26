# Setup OPNsense syslog

To enable Splunk receive logs from OPNsense VM, we need to setup both Splunk Web and OPNsense so it can communicate and transfer logs using port TCP 514

# At OPNsense Web GUI

In OPNsense terminal, you will seee ipv4 of LAN or em1. e.g: 192.168.10.254
copy this ip address (dont include netmask) and go to Host VM (Windows 10 VM in my setup)
Since host is in the same LAN, we can access web gui using that ip address
Go to any browser inside VM, paste the ip address to browser URL
Login using same credentials we use to log in OPNsense VM
Go to System > Settings > Logging
Click Remote Tab and click button Add or + sign
Next inside 'edit destination'
    Tick enabled
    Transport: TCP(4)
    Application: Click Select All
    Levels: Select All
    Facilites: Select All
    Hostname: Use Splunk Web IP address (My setup: 10.20.30.10)
    Port: 514
    rtc5424: untick or leave it default
    Click Save
Click Apply

# At Splunk Web
You can follow below link to setup the OPNsense.
The page also mentioned some addon or app for OPNsense configuration, I would recommend you to add first these two Apps (At dasboard select Apps > Find More Apps)
    OPNsense Add-on for Splunk
    OPNsense App for Splunk

After that follow the setup  to add Data Input to Splunk

[Link Here](https://netwerklabs.com/ship-opnsense-firewall-logs-to-splunk-siem/)