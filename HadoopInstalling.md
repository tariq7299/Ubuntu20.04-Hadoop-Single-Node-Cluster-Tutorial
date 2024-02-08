# **This a step by step guide of setting up an `Ubuntu` `Virtual Machine` and installing and configuring `Hadoop` (_Single-Node Cluster_) on it**

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

## Configuring Ubuntu and VM to make "Enhanced Session" mode work
-   Turn on your Ubuntu image
-   Choose `Basic Session` from the top bar
-   Log in to your Ubuntu
-   Click on `Alt + Ctrl + T` to open Ubuntu terminal
-   Go to Downloads directory: `cd ~/Downloads/`
-   Download the updated script for Ubuntu 20.04: `wget https://raw.githubusercontent.com/Hinara/linux-vm-tools/ubuntu20-04/ubuntu/20.04/install.sh`
-   Add execution permissions to the downloaded install.sh script file: `sudo chmod +x install.sh`
-   Run the script file: `sudo ./install.sh`
-   Reboot your ubuntu: `init 6`
-   Run the script again: `cd ~/Downloads/` `sudo ./install.sh`
-   Reboot one more time (just in case): `init 6`
-   Open Powershell in the host machine and then run: `Set-VM -VMName <your_vm_name>  -EnhancedSessionTransportType HvSocket`
-   Reboot your host machine
-   Open Hyber-V and head to your VM and start it
-   Make sure your are in "Enhanced Session" mode
-   You will be propmted to enter your "username" and "password" enter them and press "Ok" (the username and password of your Ubuntu image)  

ERROR: *You have encounterd a black screen*
-   Then that means that your are already logged in!, so you need to logout first!
-   To logout, start ubuntu image using `Basic Session` from the top bar of `Hyber-V` window, then login, then open terminal and then logout using `sudo pkill -KILL -u <username>` or `gnome-session-quit`
-   Then go back to "Enhanced Session" and login again!      

&nbsp;  

# Installing and Configuring Hadoop

## Some Prerequisites before installing hadoop
-   Run the following commands in Ubuntu terminal, just to ensure we already have Java 11 and SSH installed.

``` bash
sudo apt update && sudo apt upgrade -y # This will upgrade the packages in your Ubuntu
sudo apt install openssh-server openssh-client -y # This will install "openssh-server" and "openssh-clinet"
sudo apt install openjdk-11-jdk -y # This will install JDK
```   

## Create a non root user for Hadoop  

This is better for security reasons
``` bash
sudo adduser hdoop
su - hdoop
```   

## Setup SSH keys
-   Craete ssh key pair: `ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa`  
-   Copy the public key to "authorized_keys" file
-   Change some permissions:  
``` bash
chmod 600 ~/.ssh/authorized_keys
chmod 700 ~/.ssh  

ssh localhost # This will just check if ssh is correctly installed and configured
```  

##  Download and install Hadoop
-   Download hadoop: `wget https://downloads.apache.org/hadoop/common/hadoop-3.3.6/hadoop-3.3.6.tar.gz`
-   Finish the exctraction and installing by running the following commands: 
```   bash
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
``` xml
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
This is some basic configuration for "hdfs-site.xml" file:  

``` xml
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>

    <property>
        <name>dfs.namenode.name.dir</name>
        <value>file:/home/hadoop/hdfs/namenode</value>
    </property>

    <property>
        <name>dfs.datanode.data.dir</name>
        <value>file:/home/hadoop/hdfs/datanode</value>
    </property>
```

This is some basic configuration for "mapred-site.xml" file: 

``` xml
   <property>
        <name>mapreduce.cluster.local.dir</name>
        <value>/data/hdfs/tmp/mapred/local</value>
    </property>


```
*Note*: You have to manually create any directory you wrote in the `.xml` files, and maybe also assign the ownership of them to correct user!

--> for example:  
``` bash
    sudo mkdir -p /data/hdfs/tmp
    sudo mkdir -p /home/hadoop/hdfs/namenode
    sudo mkdir -p /home/hadoop/hdfs/datanode  

    # And then change there ownership to the "hdoop" user we created above  
    sudo chown -R hdoop:hdoop /data/hdfs/tmp
    sudo chown -R hdoop:hdoop /home/hadoop/hdfs
    sudo chown -R hdoop:hdoop /data/hdfs/tmp/mapred/
    sudo chown -R hdoop:hdoop /data/hdfs/tmp/mapred/local
```

## Format HDFS  
`/usr/local/hadoop/bin/hdfs namenode -format`  

*Note*:   Note that you can look through the log files to, if any thing went wrong, or you trying to solve a specific problem  

--> for example:  
``` bash
# This will display the logs of "namenode"
cat /usr/local/hadoop/logs/hadoop-hdoop-namenode-tariq-Virtual-Machine.log 
```
## Start Hadoop Services  
Launch HDFS: `/usr/local/hadoop/sbin/start-dfs.sh`  
Launch YARN: `/usr/local/hadoop/sbin/start-yarn.sh`  

## Make Sure it is running by executing  
``` bash  
jps # This will show all the running JAVA proccess, and hopfully your hadoop nodes are one of them
``` 

## You Can now access the Web interfaces of your hadoop nodes  
NameNode: http://localhost:9870  
ResourceManager: http://localhost:8088  

&nbsp;

# Lets test our Hadoop  

## First Creat `/input` dir in Hadoop HDFS  

``` bash
/usr/local/hadoop/bin/hdfs dfs -mkdir /input

```

## Create a text file with some sample text  

``` bash
# This is will create a text file with some sample text, that we later will run a mapReduce job on it!
echo -e "Hello Hadoop\nHadoop is powerful\nMapReduce is awesome\nBig Data processing\nHDFS is a distributed file system\nTesting MapReduce job\nSample input for MapReduce\nHadoop ecosystem\ndfstest dfstest2" > /usr/local/hadoop/testStringFile.txt

# Copy it to "input" dir in Hadoop HDFS
/usr/local/hadoop/bin/hdfs dfs -put /usr/local/hadoop/testStringFile.txt /input

```
## Run this command to  

``` bash
# This will run a MapReduce job using the prebuilt (default) jar file downloaded with hadoop
# And this will extract the nubmer of times this 'dfs[a-z.]+' reguler expression found in the files found in "/input"
/usr/local/hadoop/bin/hadoop jar /usr/local/hadoop/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.6.jar grep /input /output 'dfs[a-z.]+'

```

*Note*: You need to set `mapreduce.cluster.local.dir` first in `mapred-site.xml` in order to be able to use MapReduce jobs!  

*Note*: The usage to run a "MapReduce" job using command-line is like this:  

 `<path/to/hadoop/cmd/file> jar <path/to/jar/file> <mainClass (which method/type of job to run)>  <path/to/input/dir/you/want/to/run/your/MapReduce/Job/on/found/in/hadoop/hdfs/and/not/in/local/files> <path/to/output/dir/found/in/hadoop/hdfs/and/not/local/files/> [Optional (some addional args)]`  

 OR 

 ```bash
   hadoop jar /path/to/your/mapreduce.jar input_directory output_directory
   ```
---

## Now run this to test if it worked or not
``` bash
/usr/local/hadoop/bin/hdfs dfs -cat /output/*
```

*Note*: You can't write a output path that contain any already existing files or folders, so you to make sure it is empty, ot specify every time a new dir inside `/output`

--> for Example:    
``` bash
# Notice "/output/test4"
/usr/local/hadoop/bin/hadoop jar /usr/local/hadoop/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.6.jar grep /input /output/test4 'dfs[a-z.]+'
```  

##   Add `hadoop/bin` and `hadoop/sbin` to our `PATH` so we can run hadoop commands from any where/place in the terminal  
``` bash
echo 'export PATH=$PATH:/usr/local/hadoop/bin:/usr/local/hadoop/sbin' >> ~/.bashrc  
source ~/.bashrc
```  

&nbsp;  
&nbsp;  
&nbsp;  

***Congratulation! We have successfully Created an ubunutu VM and installed and configured Hadoop on it***

&nbsp;    
&nbsp;    
&nbsp;    
---  

# Some important info   

Admin username of the ubuntu VM is : tariq  
Admin Password of the ubuntu VM is : 1122
  
Hadoop username: hdoop  
Hadoop Password: 1122

ResourceManager WebInterface Access on: localhost:8088  
NameNode WebInterface Access on: localhost:9870