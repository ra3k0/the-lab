# How to install ATOR relay as Docker container

This tutorial help Atornauts test and experiment with a docker container to set up a new ATOR relay in preparation and testing for the new Ator network. This instruction works for both amd64 and arm64 (Such as Raspberry Pi).

### Install a fresh Debian 12 Bookworm (CLI Only)

https://www.debian.org/

### Make sure you are doing the install as 'root' !

```bash
sudo su
```

### Update system

```
apt-get update -y
apt-get upgrade -y
```

### Install firewall and add allow rules for SSH and ORport

```
apt install ufw -y
ufw allow 22
ufw allow 9001
ufw enable
```

## Instructions for docker and relay setup:

#### Set up Docker's apt repository

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

#### Prepare directories and fetch files

```
mkdir /opt/compose-files/
wget -O /opt/compose-files/relay.yaml https://raw.githubusercontent.com/rA3ka/the-lab/main/docker/ator-relay/relay.yaml
mkdir -p /opt/anon/etc/anon/
wget -O /opt/anon/etc/anon/anonrc https://raw.githubusercontent.com/rA3ka/the-lab/main/docker/ator-relay/anonrc
touch /opt/anon/etc/anon/notices.log
chown 100:101 /opt/anon/etc/anon/notices.log
mkdir -p /opt/anon/run/anon/
chown -R 100:101 /opt/anon/run/anon/
chmod -R 700 /opt/anon/run/anon/
mkdir -p /root/.nyx/
wget -O /root/.nyx/config https://raw.githubusercontent.com/rA3ka/the-lab/main/docker/ator-relay/config
useradd -M anond
```

#### Create and start Docker container

```
docker compose -f /opt/compose-files/relay.yaml up -d
```

#### Install NYX

```
apt-get install nyx -y
```

#### Always run Nyx with this cmd

```
nyx -s /opt/anon/run/anon/control
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

