# Server Deploying Part 2 - Deploying LXC containers and Docker Services

>[!IMPORTANT]
> This is part 2 of initial setup, and this page is covering deploying LXC containers and Docker Services, if you haven't read part 1 which is the complete configuration of the host machine, you will be able to do none of this, complete the steps listed [here](/initsetup/README.md) and come back to this page. 

## Table of Content 
* [Home](/README.md)
* Initial Setup
  * [Part 1](/initsetup/README.md)
  * Part 2 - You are here
* [Apps](/apps/README.md)


### Post LXC Creation Operations

#### Update User Repository and Installing Updates

```bash
apt update
apt upgrade
```

#### Creating User Account

By default Ubuntu Server (and most Linux Server Distributions, by extension, especially on Proxmox) will only create a `root` Superuser/Admin Account, this account is **NOT** to be used for deployment as this account will no have safety rails against human error or malicious programs (yes, viruses exist on Linux). Additionaly, it's just best practice to use a standard account for day to day operations due to a multitude of reasons but one being that the Docker service edits some core user permissions and roles which will create problems if preformed on `root`. 

TLDR: On Linux, `root` should never be used for deployment or any other day to day operations.

In order to create a standard user account and give it elevated user privileges, (`sudo` permissions), run the following command in shell

```bash
useradd [USERNAME]

```

#### Setting up Cockpit


```bash
sudo apt install cockpit
sudo systemctl enable --now cockpit.socket
```

Now you should be able to log onto the Cockpit web console uses TCP port 9090. You can access your management dashboard by navigating to `https://<lxc-ip>:9090` in any standard web browser. While the Proxmox WebUI is more than capable of any uses cases you may have for both the actual server node and any LXC containers that are running, it is still nice to have a WebUI for the actual LXC node and may prove useful for some advanced edge case scenarios such as networking for that specific LXC container.

### Setting up Docker

#### Installing Docker

Once the initial setup of the LXC container has been completed, Docker can be installed onto the server. By default, Docker is unavailable within the `apt` repository, this however doesn't mean that Docker is unable to be used in a standard Ubuntu LXC, the Docker repository just has to be added before it can be installed, which can be achieved, with the following commands.

```bash
# Add Docker's official GPG key:
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Architectures: $(dpkg --print-architecture)
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update

# Installing Docker 
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Enabling docker to launch upon boot
sudo systemctl enable docker

# Verify that Docker is running
sudo systemctl status docker
```

### Deploying Services using Docker Compose

Once Docker has been properly installed on the LXC and the actual Docker Service is properly running and flagged to launch upon boot, services can be deployed. In modern docker, there are two ways that can be used to deploy services, there is the traditional `docker` command, which while is still supported is not the most optimal way to deploy services, especially multiple services that interact with each other, such as a media stack, the most optimal way, and also the way that this lab setup uses is using `Docker Compose`.


**Example of a `Docker-Compose` `YAML` file**
```yaml
#compose.yaml

```

Now in order to deploy the services within the compose file with their configuration, use the following command:

```bash
sudo docker compose up -d #Spins up the containers
sudo docker ps #Docker Status, think ls but for docker
```

Now Docker will automatically pull the images from the web and deploy the services
