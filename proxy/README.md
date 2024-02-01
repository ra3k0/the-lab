# Configure Socks5 proxy for anon relay

### edit anon configuration
```
sudo nano /etc/anon/anonrc
```
### Add SocksPort and SocksPolicy to anon configuration
```
SocksPort 192.168.1.16:9050
SocksPolicy accept 192.168.1.0/24
```
### Add allow rule for LAN subnet
```
ufw allow from 192.168.1.0/24 proto tcp to any port 9050
```
