# For IP Blocking:

**Save the following script as:** 
` sudo nano /usr/local/bin/drop_ip.sh `
`sudo chmod +x /usr/local/bin/drop_ip.sh`

Contents:
```bash

```
**Add to crontab**

`sudo crontab -e`

`*/2 * * * * sudo /usr/local/bin/drop_ip.sh`
