## Install Debian (CLI Only).
https://www.debian.org/

## Install Docker
## Install using the apt repository.
### 1. Set up Docker's apt repository.
```
sudo apt-get update -y
sudo apt-get upgrade -y
```
```
sudo apt-get install ca-certificates curl gnupg -y
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```
#### Add the repository to Apt sources:
```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```
### 2. Install the Docker packages.
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```
### Optional, requires relogin
```
sudo usermod -aG docker $USER
```
### 3. Verify that the installation is successful by running the hello-world image:
```
docker run hello-world
```

### Prepare directories and fetch files
```
mkdir /opt/compose-files/
wget -O /opt/compose-files/ator.yaml https://raw.githubusercontent.com/rA3ka/the-lab/main/ator.yaml
mkdir -p /opt/ator/etc/tor/
wget -O /opt/ator/etc/tor/torrc https://raw.githubusercontent.com/rA3ka/the-lab/main/torrc
```

### Create and start Docker container

```
cd /opt/compose-files/
docker compose up -f /opt/compose-files/ator.yaml -d
```

# Resources:

https://www.debian.org/

https://docs.docker.com/engine/install/debian/

https://docs.docker.com/compose/install/linux/#install-using-the-repository

https://hub.docker.com/r/svforte/ator-protocol

https://github.com/ATOR-Development/ator-protocol/blob/main/docker/docker-compose.yaml



