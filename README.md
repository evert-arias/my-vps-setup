# My personal VPS setup

This repository aim to host my personal VPS configuration as well as docker setup.

* [Traefik](https://traefik.io/) (Reverse proxy and load balancer)
* [Portainer](https://www.portainer.io/) (Container management GUI for docker)
* [OpenVPN](https://github.com/kylemanna/docker-openvpn) (Virtual Private Network)
* [Mosquitto](https://mosquitto.org/) (MQTT broker)
* [NodeRed](https://nodered.org/) (Programming tool for wiring together hardware devices)
* [Verdaccio](https://verdaccio.org/) (Open source private npm proxy registry)



## Pre-setup requirements

Install the following tools when running in a fresh VPS instance.
#### NodeJS and NPM
Install node version manager (NVM) by typing the following at the command line.
```bash
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
```
Activate NVM by typing the following at the command line.
```bash
$ . ~/.nvm/nvm.sh
```
Use NVM to install the latest version of Node.js by typing the following at the command line.
```bash
$ nvm install node
```
#### Docker
```bash
$ sudo yum update -y
$ sudo yum install docker
# Start the Docker service.
$ sudo service docker start
# Add the ec2-user to the docker group so you can execute Docker commands without using sudo.
$ sudo usermod -a -G docker ec2-user
# Make docker auto-start.
$ sudo chkconfig docker on
```
#### Docker-compose
Copy the appropriate docker-compose binary from GitHub.
```bash
$ sudo curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
```
Fix permissions after download.
```bash
$ sudo chmod +x /usr/local/bin/docker-compose
```
#### Git
Because its always needed.
```bash
$ sudo yum install -y git
```

-------------------------------------------

Reboot to verify it all loads fine on its own.

```bash
$ sudo reboot
```



## Docker setup

#### Clone repository
```bash
$ git clone git@github.com:evert-arias/my-vps-docker-setup.git
$ cd my-vps-docker-setup
```
#### Traefik initial setup
Create the acme.json file where traefik will store the certificates obtained from Let's Encrypt.

```bash
# Create traefik/data folder
$ mkdir traefik traefik/data
# Create acme.json file
$ touch traefik/data/acme.json
# Chmod 600 permission
$ chmod 600 traefik/data/acme.json
```

#### Verdaccio initial setup
Create verdaccio folder and chmod 777 permission
```bash
$ mkdir ./verdaccio
$ chmod 777 ./verdaccio
```

#### OpenVPN initial setup
Initialize the configuration files and certificates
```bash
$ docker-compose run --rm openvpn ovpn_genconfig -u udp://vpn.earias.me
$ docker-compose run --rm openvpn ovpn_initpki
```
Fix ownership (depending on how to handle your backups, this may not be needed)
```bash
$ sudo chown -R $(whoami): ./openvpn-data
```

#### Environment variables
Create .env file on the repository's root directory. Add the following environment variables to the .env file.

```bash
# General environment variables
SERVER_DOMAIN=earias.me
RESTART_MODE=unless-stopped

# Traefik environment variables
TRAEFIK_SUBDOMAIN=traefik
# Traefik web username and password
# Ex: admin:$2y$05$H3Vqg6NdEihJMyuOcq.ZzOur0PgtBap8RwPSIayAhDvjSkZdgtrwG 
# The password can be encoded in MD5, SHA1 and BCrypt: you can use htpasswd to generate them.
# Example: echo $(htpasswd -nbB admin "password")
TRAEFIK_USER_PASS=admin:password

# Traefik ACME_CASERVER
# The default ca server is: https://acme-v02.api.letsencrypt.org/directory. 
# For testing use: https://acme-staging-v02.api.letsencrypt.org/directory
TRAEFIK_ACME_CASERVER=https://acme-v02.api.letsencrypt.org/directory
# Email address used for Let's Encrypt
TRAEFIK_ACME_EMAIL=me@server.com

# Portainer environment variables
PORTAINER_SUBDOMAIN=portainer

# NodeRed environment variables
NODERED_SUBDOMAIN=nodered

# Mosquitto environment variables
MOSQUITTO_SUBDOMAIN=mqtt
MOSQUITTO_USER=user
MOSQUITTO_PASS=password

# Verdaccio environment variables
VERDACCIO_SUBDOMAIN=npm
```

#### Execute
Finally execute the following docker-compose command to run.

```bash
$ docker-compose up -d
```



## Copyright

2021 © Evert Arias