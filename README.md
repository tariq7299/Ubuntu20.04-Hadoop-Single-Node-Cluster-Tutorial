# **This a demo of setting up a Ubuntu VM with (Hadoop, Ranger) installed on it**

# VM setup on Windows 10

## Creating Ubuntu image and setting up VM using Hyber-V
-   Search for "Hyber-V" using windows search bar
-   Enable "Hyber-V" checkbox then press ok
-   Restart your PC
-   Search for "Hyber-V" using windows search bar again
-   Click on "Hyper-V Quick Create"
-   Choose Ubuntu 20.04 LTS
-   Then press on Create Virtual Machine
-   Wait and... DONE !! Now hopfully Your Ubuntu has been installed successfully!

## Confugring Ubunto and VM to make "Enhanced Session" mode work
-   Turn on your Ubuntu image
-   Click on "Basic Session" on the top tool bar
-   Log in to your Ubuntu image
-   Click on "Alt" + Ctrl + T to open Ubuntu terminal
-   Go to Downloads directory: `cd ~/Downloads/`
-   Download the updated script for Ubuntu 20.04: `wget https://raw.githubusercontent.com/Hinara/linux-vm-tools/ubuntu20-04/ubuntu/20.04/install.sh`
-   Add execution permissions to the downloaded install.sh script file: `sudo chmod +x install.sh`
-   Run the script file: `sudo ./install.sh`
-   Reboot your ubuntu: `init 6`
-   Run the script again: `cd ~/Downloads/` `sudo ./install.sh`
-   Open Powershell in the host machine and then run: `Set-VM -VMName <your_vm_name>  -EnhancedSessionTransportType HvSocket`
-   Reboot your host machine
-   Open Hyber-V and head to your VM and start it
-   Make sure your are in "Enhanced Session" mode
-   You will be propmted to enter your "username" and "password" enter them and press "Ok" (the username and password of your Ubuntu image)
*If you encounter a black screen*
-   Then that means that your are already logged in!, so you need to logout first!
-   To logout, start ubuntu image using "Basic Session", then open terminal and then logout using `sudo pkill -KILL -u <username>`
-   Then go back to "Enhanced Session" and login again!

# Installing and Configuring Hadoop

## Some Prerequisites before installing hadoop
-   Run the following commands in Ubuntu terminal, just to ensure we already have Java 11 and SSH installed.

```
sudo apt update && sudo apt upgrade -y # This will upgrade the packages in your Ubuntu
sudo apt install openssh-server openssh-client -y # This will install "openssh-server" and "openssh-clinet"
sudo apt install openjdk-11-jdk -y # This will install JDK
```  

## Create a non root user for Hadoop  

```  
sudo adduser hdoop
su - hdoop
```  

## Setup SSH keys
-   Craete ssh key pair: `ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa`  
-   Copy the public key to "authorized_keys" file
-   Change some permissions:
```
chmod 600 ~/.ssh/authorized_keys
chmod 700 ~/.ssh
```  
##  Download and install Hadoop
-   Download hadoop: `wget https://downloads.apache.org/hadoop/common/hadoop-3.3.6/hadoop-3.3.6.tar.gz`
-   Finish the exctraction and installing by running the following commands: 
```  
 tar xzf hadoop-3.3.6.tar.gz  
 sudo mv hadoop-3.3.6 /usr/local/hadoop  
 sudo chown -R hdoop:hdoop /usr/local/hadoop
```
## Configure Hadoop Environment   
-   Run this command to set the "JAVA_HOME" env variable: `echo 'export JAVA_HOME=$(readlink -f /usr/bin/java | sed "s:bin/java::")' | sudo tee -a /usr/local/hadoop/etc/hadoop/hadoop-env.sh`  

##  Edit Configuration Files
-   Add your cluster configurations in "core-site.xml" file: `nano /usr/local/hadoop/etc/hadoop/core-site.xml`  
  
*Some helpfull references to configure your cluster*  
[Hadoop Configuration Documentaion](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/ClusterSetup.html)  
[Hadoop Configuration Blog](https://www.edureka.co/blog/explaining-hadoop-configuration/)  

This is some basic configuration for "core-site.xml" file:  
```
<property>
		<name>hadoop.tmp.dir</name>
		<value>/data/hdfs/tmp</value>
		<description>Where Hadoop will place all of its working files</description>
	</property>
	<property>
		<name>fs.defaultFS</name>
		<value>hdfs://localhost:9000</value>
		<description>Where HDFS NameNode can be found on the network</description>
	</property>
```

## Format HDFS  
`/usr/local/hadoop/bin/hdfs namenode -format`

## Start Hadoop Services  
Launch HDFS: `/usr/local/hadoop/sbin/start-dfs.sh`  
Launch YARN: `/usr/local/hadoop/sbin/start-yarn.sh`  


---
# Some important info   

Admin username of the ubuntu VM is : tariq  
Admin Password of the ubuntu VM is : SsarhanS7299  
  
Hadoop username: hdoop  
Hadoop Password: 1122