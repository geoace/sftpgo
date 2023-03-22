# Setting up the VM

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

`echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null`
  
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
`export GCSFUSE_REPO=gcsfuse-\`lsb_release -c -s\``
`echo "deb https://packages.cloud.google.com/apt $GCSFUSE_REPO main" | sudo tee /etc/apt/sources.list.d/gcsfuse.list`
`curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -``

Type the following and then follow the URL and auth code prompts...

`gcloud auth login` 







