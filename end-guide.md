# Expected Outcome

Inside Splunk Web, Under Search & Reporting, you should able to see logs from Host VM and OPNsense

At search bar,  insert

for Host VM (put inside "" the name of the your host PC ): 
    windows host="DESKTOP-VNED4CP"

for OPNsense:
    source="tcp:514" index="opnsense"   sourcetype="opnsense:syslog"

