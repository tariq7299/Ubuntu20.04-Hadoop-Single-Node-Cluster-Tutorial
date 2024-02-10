# Installing And Configuring *Ranger*  

## Some Prerequistes before we actually install *Ranger*  

### Installing Docker

Before you install Docker Engine for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.  

1-  **Set up Docker's apt repository.**

``` bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Just quickly update again your recources list
sudo apt-get update
```   

2-  **Install the Docker packages**   
To install the latest version, run: `sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin`  
  
3-  **Verify that the Docker Engine installation is successful by running the hello-world image**    
`sudo docker run hello-world`  


### Installing *Docker Compose (V1)* (the standalone version and not the plugin!)  

1- Install the latest version of docker-compose:
` sudo curl -SL https://github.com/docker/compose/releases/download/v2.24.5/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose`  

2-  Apply executable permissions to the standalone binary in the target path for the installation.  
`sudo chmod  +x /usr/local/bin/docker-compose`  

3-  Test and execute compose commands using `docker-compose`


## Install *Apache Ranger*  

1- Download *Apache Ranger*:  

``` bash
mkdir -p ${HOME}/git
cd ${HOME}/git
git clone https://github.com/apache/ranger.git
```
2-  Bring up Apache Ranger (Builds if needed)  

``` bash
cd ${HOME}/git/ranger
# Enable only necessary services to be run along with CORE ranger services
export ENABLED_RANGER_SERVICES="tagsync,hadoop,hbase,kafka,hive,knox,kms"
# Execute this command to bring the services up (after successful build if it is not already build)
./ranger_in_docker up
```


&nbsp;
&nbsp;  
&nbsp;  
  

# ***FAILD HERE!!!***
# Potentional reasons:
  - Some thing is wrong in the ranger code itself!  
