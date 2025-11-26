# Splunk Setup

Splunk by default use IP localhost or 127.0.0.1 but VMs cant use that ip since Splunk Web or Indexer are on outside of the VMs network (more specifically at local machine)
We need to change the default ip to ip address we have assigned to newly created virtual adapter (Mine is 10.20.30.10)

First find Splunk Folder (Usually at Program Files)

At Splunk\etc\system\local folder
    Open web.conf
    Add this in the file and save it

        [settings]
        mgmtHostPort = <YOUR-IP>:8089
        # Example
        mgmtHostPort = 10.20.30.10:8089

At D:\Program Files\Splunk\etc folder
    Open splunk-launch.conf
    Add this in the file and save it

    SPLUNK_BINDIP=<YOUR-IP>

    # Example
    SPLUNK_BINDIP=10.20.30.10


After that run the splunk and login with new ip: <YOUR-IP>:8000



## Setup Splunk Unverisal Forwarder in Host VM

Use the newly created virtual network adaptor IP Address as receiver IP 
Follow below Youtube video to setup Forwarder inside Host VM

For Deployment Server, it is optional to put IP address and port but the Receiving Indexer must be specified.

Click the thumbnail to open the forwarder setup video on YouTube:

[![Splunk Universal Forwarder Setup](https://img.youtube.com/vi/Uvxt3MeZN0c/hqdefault.jpg)](https://www.youtube.com/watch?v=Uvxt3MeZN0c)