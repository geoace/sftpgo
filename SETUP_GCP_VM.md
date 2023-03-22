# Setting up the VM
Note: When you create the VM, make sure to allow access to all APIs or at a minimum the Storage API.

### Install git

`sudo apt update`

`sudo apt-get install git --yes`

### Install Docker. Based off of: https://docs.docker.com/engine/install/debian/

#### Set up the repository

`sudo apt-get update`

`sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release`
    
`sudo mkdir -m 0755 -p /etc/apt/keyrings`

`curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg`

`echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null`
  
#### Install docker engine

`sudo apt-get update`

`sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin`

Verify installation:

`sudo docker run hello-world`

#### Run docker without sudo. Based on: https://docs.docker.com/engine/install/linux-postinstall/

`sudo groupadd docker` 

`sudo usermod -aG docker $USER`

`newgrp docker`

`docker run hello-world`

# Install SFTP

## Mount Google Storage Bucket. Based on: https://github.com/GoogleCloudPlatform/gcsfuse

Install gcsfuse. Follow: https://github.com/GoogleCloudPlatform/gcsfuse/blob/master/docs/installing.md

Type the following and then follow the URL and auth code prompts...

`gcloud auth login` 

`mkdir /home/aaron/lizmount`

`gcsfuse lizmount-1 /home/aaron/lizmount`

Check to make sure it mounted:

`df -h`

#### NOTE: The mounted bucket will NOT show previously-created directories within the SFTP server unless you create the directories in name only. Once you do this, you'll be able to see the files within.

# Installing SFTPGo. Based on https://github.com/geoace/sftpgo/tree/main/docker

`docker pull drakkan/sftpgo`

`chown -R 1000:1000 /home/aaron/lizmount`

Note that port was changed from default below to ensure there are no conflicts with lizmap on the load balancer.

`sudo apt-get install ufw`

`sudo ufw allow 8050 && sudo ufw allow 2022`

`docker run --name enterprise-sftp \
    -p 8050:8050 \
    -p 2022:2022 \
    --mount type=bind,source=/home/aaron/enterprise-data,target=/srv/sftpgo \
    -e SFTPGO_HTTPD__BINDINGS__0__PORT=8050 \
    -d "drakkan/sftpgo"`

One line for copy/paste:

`docker run --name enterprise-sftp -p 8080:8090 -p 2022:2022 --mount type=bind,source=/home/aaron/enterprise-data,target=/srv/sftpgo -e SFTPGO_HTTPD__BINDINGS__0__PORT=8090 -d "drakkan/sftpgo"`







