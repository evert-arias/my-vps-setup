# My personal VPS setup

## Required tools
Install the following tools when running in a fresh VPS instance.
### NodeJS and NPM
Install node version manager (NVM) by typing the following at the command line.
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
```
Activate NVM by typing the following at the command line.
```bash
. ~/.nvm/nvm.sh
```
Use NVM to install the latest version of Node.js by typing the following at the command line.
```bash
nvm install node
```
### Docker
```bash
sudo yum update -y
sudo yum install docker
// Start docker deamon.
sudo service docker start
// Make docker auto-start.
sudo chkconfig docker on
```
### Docker-compose
Copy the appropriate docker-compose binary from GitHub.
```bash
sudo curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
```
Fix permissions after download.
```bash
sudo chmod +x /usr/local/bin/docker-compose
```
### Git
Becouse its always needed.
```bash
sudo yum install -y git
```

Reboot to verify it all loads fine on its own.

```bash
sudo reboot
```

## Docker setup

### Clone repository
```bash
$ git clone git@github.com:evert-arias/my-vps-docker-setup.git
$ cd my-private-network
```
