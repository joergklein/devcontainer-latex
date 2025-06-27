## Run Coder on Virtual Machine

**Notice: The configuration of this VM is not powerful enough to compile TeX Live “scheme full”!**

| Typ        | Virtual Machine |
| :--------- | :-------------- |
| Size       | vps 4 8 240     |
| CPU        | 4 vCore         |
| RAM        | 8 GB            |
| Disc space | 240 GB NVMe SSD |

## Requirements

- [Docker](#install-docker)
- [Docker Compose](#install-docker-compose)
- [Coder](#install-coder)

## Install Docker

Docker provides a way to run applications securely isolated in a container, packaged with all its dependencies and libraries. At first you have to install Docker on your computer. I have covered the installation steps for setting up Docker packages on AlmaLinux using the command terminal in this tutorial.

### Add Docker Repository

Just like the previous version of AlmaLinux 8, the 9 also not offers Docker’s latest packages to install. So, we manually run the given command to enable the Docker repository on AlmaLinux 9.

```sh
sudo dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo
```

### Docker installation

Once the system is ready, we can execute the main installation command to configure Docker CE on AlmaLinux 9. This will install the dependencies and extra packages we required to run this open-source container service on RHEL-based Linux systems.

```sh
sudo dnf install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

### Start Docker Service

Next, we start and enable the Docker service and after that, we will go through a command that will let’s know whether it is working fine without any errors or not.

```sh
sudo systemctl enable --now docker
```

To check the service status:

```sh
sudo systemctl status docker
```

Output:

```text
systemctl status docker
● docker.service - Docker Application Container Engine
	  Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; preset: disabled)
	  Active: active (running) since Sun 2023-05-21 13:56:26 UTC; 19h ago
TriggeredBy: ● docker.socket
		 Docs: https://docs.docker.com
	Main PID: 13344 (dockerd)
		Tasks: 101
	  Memory: 587.8M
		  CPU: 1min 22.835s
	  CGroup: /system.slice/docker.service
				 ├─13344 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
				 ├─13749 /usr/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 9000 -container-ip 172.17.0.2 -container-port 9000
				 ├─13758 /usr/bin/docker-proxy -proto tcp -host-ip :: -host-port 9000 -container-ip 172.17.0.2 -container-port 9000
				 ├─13771 /usr/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 8000 -container-ip 172.17.0.2 -container-port 8000
				 ├─13779 /usr/bin/docker-proxy -proto tcp -host-ip :: -host-port 8000 -container-ip 172.17.0.2 -container-port 8000
				 ├─16283 /usr/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 443 -container-ip 172.18.0.3 -container-port 443
				 ├─16292 /usr/bin/docker-proxy -proto tcp -host-ip :: -host-port 443 -container-ip 172.18.0.3 -container-port 443
				 ├─16308 /usr/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 81 -container-ip 172.18.0.3 -container-port 81
				 ├─16316 /usr/bin/docker-proxy -proto tcp -host-ip :: -host-port 81 -container-ip 172.18.0.3 -container-port 81
				 ├─16331 /usr/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 80 -container-ip 172.18.0.3 -container-port 80
				 └─16337 /usr/bin/docker-proxy -proto tcp -host-ip :: -host-port 80 -container-ip 172.18.0.3 -container-port 80
```

### Add non-root user to Docker group

By default to run the Docker command, we need to use the sudo every time with it. To remove it, we can add our current user to the Docker group. After that, we can run the Docker command tool to create containers without the sudo rights.

```sh
sudo usermod -aG docker $USER
newgrp docker
```

## Install Docker Compose

Docker Compose is a useful tool for running multi-containers Docker applications. Using Docker Compose, we can configure the application’s services in a YAML file that helps you to create and start all services from the defined configurations. It allows different users to launch, run, communicate and close containers using a just single coordinated command.

Docker Compose works in all environments: production, staging, development, testing, etc. It offers different features that are listed below:

- It allows to create and execute multiple isolated environments on a single host
- It preserves volume data used by your services or when containers are created.
- It can reuse the existing containers which is also an effective feature.
- Using Docker Compose, we can use different environment variables in a configuration file and can customize the composition between different environments.

### Download Docker Compose

Use curl to download the Compose file into the /usr/bin directory.

```sh
curl -L "https://github.com/docker/compose/releases/download/v2.37.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose
```

Output:

```text
% Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
																 Dload  Upload   Total   Spent    Left  Speed
	0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
	100 42.8M  100 42.8M  0     0  45.1M      0 --:--:-- --:--:-- --:--:-- 58.2M
```

Next, set the correct permissions so that the docker-compose command is executable.

```sh
chmod +x /usr/bin/docker-compose
```

Verify the installation.

```
docker-compose --version
```

Output:

```sh
Docker Compose version v2.37.1
```

## Install Coder

Bevor you install coder from https://coder.com you have to add a DNS A-Record for Coder. Maybe you use a Proxy, you need an entry and a certificate for the subdomain.

|  Typ  | Hostname | IP-Address      |
| :---: | -------- | --------------- |
|   A   | coder    | xxx.xxx.xxx.xxx |

Open a Port with the port number: `7080` in your firewall.

### Here is my docker-compose.yaml for Coder.

Update group_add: in docker-compose.yaml with the gid of docker group. You can get the docker group gid by running the below command:

```sh
getent group docker | cut -d: -f3
```
Change the line `group_add: 987` to your docker group id. Please check out the Coder documentation: [install coder via docker-compose](https://coder.com/docs/install/docker#install-coder-via-docker-compose).

```yml
version: "3.9"
services:
  coder:
    # This MUST be stable for our documentation and
    # other automations.
    image: ghcr.io/coder/coder:${CODER_VERSION:-latest}
    ports:
      - "7080:7080"
    restart: always
    environment:
      CODER_PG_CONNECTION_URL: "postgresql://${POSTGRES_USER:-username}:${POSTGRES_PASSWORD:-password}@database/${POSTGRES_DB:-coder}?sslmode=disable"
      CODER_HTTP_ADDRESS: "0.0.0.0:7080"
      # You'll need to set CODER_ACCESS_URL to an IP or domain
      # that workspaces can reach. This cannot be localhost
      # or 127.0.0.1 for non-Docker templates!
      CODER_ACCESS_URL: "https://coder.example.com"
    # If the coder user does not have write permissions on
    # the docker socket, you can uncomment the following
    # lines and set the group ID to one that has write
    # permissions on the docker socket.
    group_add:
      - "987" # docker group on host
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      # Run "docker volume rm coder_coder_home" to reset the dev tunnel url (https://abc.xyz.try.coder.app).
      # This volume is not required in a production environment - you may safely remove it.
      # Coder can recreate all the files it needs on restart.
      - coder_home:/home/coder
    depends_on:
      database:
        condition: service_healthy
  database:
    # Minimum supported version is 13.
    # More versions here: https://hub.docker.com/_/postgres
    image: "postgres:16"
    ports:
      - "5432:5432"
    restart: always
    environment:
      POSTGRES_USER: ${POSTGRES_USER:-username} # The PostgreSQL user (useful to connect to the database)
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD:-password} # The PostgreSQL password (useful to connect to the database)
      POSTGRES_DB: ${POSTGRES_DB:-coder} # The PostgreSQL default database (automatically created at first launch)
    volumes:
      - coder_data:/var/lib/postgresql/data # Use "docker volume rm coder_coder_data" to reset Coder
    healthcheck:
      test:
        [
          "CMD-SHELL",
          "pg_isready -U ${POSTGRES_USER:-username} -d ${POSTGRES_DB:-coder}",
        ]
      interval: 5s
      timeout: 5s
      retries: 5
volumes:
  coder_data:
  coder_home:
```
- Start Coder with `docker compose up -d`
- Now coder can be reached with the subdomain `coder.example.com`.

