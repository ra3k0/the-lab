# Configure Socks5 proxy for anon relay

### Edit anon configuration
```
sudo nano /etc/anon/anonrc
```
### Add SocksPort and SocksPolicy to anon configuration
```
SocksPort 192.168.1.10:9050
SocksPolicy accept 192.168.1.0/24
```
### Add allow rule for LAN subnet to SocksPort
```
ufw allow from 192.168.1.0/24 proto tcp to any port 9050
```
### Configure proxy for browser
<figure><img src="./Firefox connection settings.png" alt="" width="475"><figcaption></figcaption></figure>
