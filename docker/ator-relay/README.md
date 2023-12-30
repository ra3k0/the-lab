# How to install ATOR relay as Docker container 
#### Not for use on arm64 devices
This tutorial help Atornauts test and experiment with a docker container to set up a new ATOR relay in preparation and testing for the new Ator network

## Install a fresh Debian 12 Bookworm (CLI Only)

https://www.debian.org/

## Make sure you are doing the install as 'root' !
```
sudo su
```

## Instructions for docker and relay setup:
#### Set up Docker's apt repository
```
apt-get update -y
apt-get upgrade -y
```
```
apt-get install ca-certificates curl gnupg -y
install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg
chmod a+r /etc/apt/keyrings/docker.gpg
```
#### Add the repository to Apt sources:
```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  tee /etc/apt/sources.list.d/docker.list > /dev/null
apt-get update
```
#### Install the Docker packages
```
apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```
<!--### Optional, requires relogin
```
usermod -aG docker $USER
```-->
#### Verify that the installation is successful by running the hello-world image:
```
docker run hello-world
```

#### Prepare directories and fetch files
```
mkdir /opt/compose-files/
wget -O /opt/compose-files/ator.yaml https://raw.githubusercontent.com/rA3ka/the-lab/main/docker/ator-relay/ator.yaml
mkdir -p /opt/ator/etc/tor/
wget -O /opt/ator/etc/tor/torrc https://raw.githubusercontent.com/rA3ka/the-lab/main/docker/ator-relay/torrc
touch /opt/ator/etc/tor/notices.log
chown 100:101 /opt/ator/etc/tor/notices.log
mkdir -p /opt/ator/run/tor/
chown -R 100:101 /opt/ator/run/tor/
chmod -R 700 /opt/ator/run/tor/
mkdir -p /root/.nyx/
wget -O /root/.nyx/config https://raw.githubusercontent.com/rA3ka/the-lab/main/docker/ator-relay/config
useradd -M atord
```

#### Create and start Docker container
```
docker compose -f /opt/compose-files/ator.yaml up -d
docker ps
```

#### Install NYX
```
apt-get install nyx -y
```
#### Always run Nyx with this cmd
```
nyx -s /opt/ator/run/tor/control
```


## Done!

#### Update relay to run latest version
```
docker container rm --force ator-relay
docker pull svforte/ator-protocol:latest
docker compose -f /opt/compose-files/ator.yaml up -d
```

### Commands for testing and monitoring:

#### Start nyx with control file
```
nyx -s /opt/ator/run/tor/control
```
#### Check systemctl logs for Tor service
```
docker logs ator-relay
```
#### Monitor Tor log
```
tail -f /opt/ator/etc/tor/notices.log
```
#### Restart the relay container
```
docker restart ator-relay
```
#### Remove the relay container
```
docker rm ator-relay --force
```

## Resources:

https://www.debian.org/

https://docs.docker.com/engine/install/debian/

https://hub.docker.com/r/svforte/ator-protocol

https://github.com/ATOR-Development/ator-protocol/blob/main/docker/docker-compose.yaml

https://github.com/ATOR-Development/ator-protocol/blob/main/docker/config/torrc-example

https://github.com/ATOR-Development/ator-protocol/blob/11f734c0a9df1bc6b2316d70da834a77224a9805/docker/config/torrc-example

## Collaborators

<table>
  <tbody>
    <tr>
     <td align="center" valign="top" width="14.28%"><a href="https://github.com/cl0ten"><img src="https://avatars.githubusercontent.com/u/143603910?v=4" width="100px;" alt="cl0ten"/><br /><b>cl0ten</b></a><br /></td>
          </tr>
  </tbody>
</table>

