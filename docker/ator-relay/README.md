# How to install ATOR relay as Docker container for Debian & Ubuntu

This tutorial help Atornauts test and experiment with a docker container to set up a new ATOR relay in preparation and testing for the new Ator network. This instruction works for both amd64 and arm64 (Such as Raspberry Pi).


### Update system and install packages

```
sudo apt-get update -y
sudo apt-get upgrade -y
sudo apt-get install nyx -y
sudo apt-get install ufw -y
```

### Enable firewall and add allow rules for SSH and ORport

```
sudo ufw allow 22
sudo ufw allow 9001
sudo ufw enable
```

## Instructions for docker and relay setup:

#### Set up Docker's apt repository

```
sudo apt-get install ca-certificates curl gnupg -y 
sudo install -m 0755 -d /etc/apt/keyrings
. /etc/os-release
sudo curl -fsSL https://download.docker.com/linux/$ID/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
sudo echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/$ID \
  $(. /etc/os-release && sudo echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

#### Install the Docker packages

```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```

#### Prepare directories and fetch files

```
sudo mkdir /opt/compose-files/
sudo mkdir -p /opt/anon/etc/anon/
sudo mkdir -p /opt/anon/run/anon/
sudo mkdir -p /root/.nyx/
sudo chmod -R 700 /opt/anon/run/anon/
sudo chown -R 100:101 /opt/anon/run/anon/
sudo touch /opt/anon/etc/anon/notices.log
sudo chown 100:101 /opt/anon/etc/anon/notices.log
sudo useradd -M anond
sudo wget -O /opt/compose-files/relay.yaml https://raw.githubusercontent.com/rA3ka/the-lab/main/docker/ator-relay/relay.yaml
sudo wget -O /opt/anon/etc/anon/anonrc https://raw.githubusercontent.com/rA3ka/the-lab/main/docker/ator-relay/anonrc
sudo wget -O /root/.nyx/config https://raw.githubusercontent.com/rA3ka/the-lab/main/docker/ator-relay/config
```

#### Create and start Docker container

```
sudo docker compose -f /opt/compose-files/relay.yaml up -d
```

#### Start Nyx to monitor the Relay

```
sudo nyx -s /opt/anon/run/anon/control
```

## Done!

### Commands for updating, testing and monitoring:

#### Edit relay configuration

```
nano /opt/anon/etc/anon/anonrc
```

#### Update relay to run latest version

```
docker container rm --force ator-relay
docker pull svforte/anon-stage:latest
docker compose -f /opt/compose-files/relay.yaml up -d
```

#### Start nyx with control file

```
nyx -s /opt/anon/run/anon/control
```

#### Check systemctl logs for Tor service

```
docker logs ator-relay
```

#### Monitor Tor log

```
tail -f /opt/anon/etc/anon/notices.log
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

https://hub.docker.com/u/svforte

https://github.com/ATOR-Development/ator-protocol/blob/main/docker/docker-compose.yaml

https://github.com/ATOR-Development/ator-protocol/blob/main/docker/config/anonrc-example

## Creators

<table>
  <tbody>
    <tr>
       <td align="center" valign="top" width="14.28%"><a href="https://github.com/ra3ka"><img src="https://avatars.githubusercontent.com/u/72023964?v=4" width="100px;" alt="ra3ka"/><br /><b>rA3ka</b></a><br /></td>
       <td align="center" valign="top" width="14.28%"><a href="https://github.com/cl0ten"><img src="https://avatars.githubusercontent.com/u/143603910?v=4" width="100px;" alt="cl0ten"/><br /><b>cl0ten</b></a><br /></td>
    </tr>
  </tbody>
</table>

