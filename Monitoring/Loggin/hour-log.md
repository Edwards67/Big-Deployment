# For logging:

`sudo nano /usr/local/bin/hourly_capture.sh`
`sudo chmod +x /usr/local/bin/hourly_capture.sh`
```bash
#!/bin/bash

# Define the paths for the log files with timestamps
timestamp=$(date +\%Y-\%m-\%d-\%H)
capture_log="/var/log/honeypot/honeypot-$timestamp.log"

# Run tcpdump in the background and capture packets ignore many ports that the reverse proxy uses, and ignore ports that are talking to our backend. Trying to limit the ouput of the TCPDump as much as possible
sudo tcpdump -i any 'tcp[tcpflags] & (tcp-syn) != 0 and tcp[tcpflags] & (tcp-ack) == 0 and ip and inbound and not ip6 and not port 22 and not port 80 and not port 42725 and not port 138 and not src host 127.0.0.1 and not (dst net 255.255.255.255 or src net 255.255.255.255) and not arp and not icmp and not (src net 10.152.183.1 or dst net 10.152.183.1) and not (src net 10.1.236.60 or dst net 10.1.236.60) and not (src net 10.102.67.53 or dst net 10.102.67.53) and not port 137 and not port 8181 and not port 8080 and not port 443 and not (src net 192.168.122.213 and dst net 192.168.122.213)' -n -tttt -q
TCPDUMP_PID=$!

# Sleep for 58 minutes
sleep 3480

# Kill 
sudo kill $TCPDUMP_PID

```
`cat /var/log/honeypot/honeypot-$(date +\%Y-\%m-\%d-\%H).log`

**Add to crontab**
`0 * * * * sudo /usr/local/bin/hourly_capture.sh`