# **This a step by step guide of setting up an `Ubuntu` `Virtual Machine` and installing and configuring `Hadoop` (_Single-Node Cluster_) on it**

# VM setup on mac (Apple Silicon)
**NOTE** If you are using Windows, go to *VM setup on Windows*  
**NOTE** The only availble Ubuntu releases prior to 23.04 for apple silicon mac book devices is server editions only !, and we need desktop editions for our setup, so we got two valid options:
    -A  Install any Ubuntu arm64 desktop edition later than 22.04
    -B  Install Ubunutu arm64 server edition prior to 23.04 and then convert it to desktop edition
And we have choose option *B* in our guide!

1-  Download *VMware player* from this link [Download VMware Player](https://customerconnect.vmware.com/en/evalcenter?p=fusion-player-personal-13)  
But you have to create an account first !  

2-  Install it  

3-  Download *Ubuntu 22.04 LTS-server* *.iso* file from this link [Download Ubuntu 22.04 arm64](https://ubuntu.com/download/server/arm)(this a compatible version with apple silicon)  

4-  Press the `+` sign on the top left  then `New`  

5-  Drag your `.iso` file then press `continue`

6-  Press `Customize Settings` then choose a location  

7-  Press `Processors & Memory` then choose `4 processor cores`  

8-  Press on `Hard Disk` then expand it to 50 GB, then click `apply`  

9-  Then exit this settings window and then click on the play button  

10- Then let it load then choose the language  

11- Press `Continue without updating`

12- Press `Done`  

13- Choose `Ubuntu Server` then `Done`  

14- Choose `ens160` then click `Done`  

15- Don't type anything in proxy addess, and just click on `Done`  

16- Click `Done` again  

17- Click `Done`again  

18- In `Storage configuration` we want to resize the filesystem drive to take the whole 50 GB, we have allocated to this VM:
    -   Under `FILE SYSTEM SUMMARY` `Unmount` `/` 
    -   Then under `USED DEVICES` click on `ubuntu 22.04....` and click `Delete`  
    -   Then click on `free space` and then create a new logincal drive using all of the available space  

19- Then type your info  then `Done`  

20- Skip installing ubuntu pro and then Click `Done`  

21- Don't Choose `Install OpenSSh server` then click `Done`  

22- Then it will install the OS then click `Reboot now`

23- Then execute this
```bash
sudo apt -y update && sudo apt -y upgrade  
```
24- FINISHED !

## Convert arm64 Ubuntu 22.04 LTS server edition to desktop edition 

1.  Install Ubuntu desktop package: `sudo apt install ubuntu-desktop`  

2.  Install open-vm-tools-desktop package `sudo apt install open-vm-tools-desktop`  

3.  Install Ubuntu Software from the Snap store `sudo snap install snap-store`  

4.  Disable the use of `systemd-networkd` and replces it by `NetworkManager`
**NOTE** This is necessary because Ubuntu desktop edition and some of its apps and services uses `NetworkManager`
```bash
sudo systemctl disable systemd-networkd.service
sudo systemctl mask systemd-networkd.service
sudo systemctl stop systemd-networkd.service
```
5.  Save a copy of the .yaml file found inside the /etc/netplan directory `cp /etc/netplan/*.yaml ./`  

6.  Edit the .yaml file found inside the /etc/netplan directory (there should be only one, and its name may vary between Ubuntu releases). Use the editor of your choice; you will need to execute the editor with sudo as the .yaml file is owned by root. 
```bash
# Example using vim text editor
cd /etc/netplan
sudo vim 00-installer-config.yaml
``` 

7.  Replace the entire contents of the .yaml file with the following:
```YAML
network:
version: 2
renderer: NetworkManager
```

8.  Set the system to use NetworkManager for networking management.
```bash
sudo netplan generate
sudo systemctl unmask NetworkManager
sudo systemctl enable NetworkManager
sudo systemctl start NetworkManager
```

10.  Update boot files `sudo update-initramfs -u -k all`

11. Reboot the VM
```bash
sudo systemctl reboot
#Upon reboot, the VM should display a graphical login. Logging in will now start a graphical session
```  

## Guide on how to setup Remote access to your VM

This is necessary to make copy paste work between the Guest os and the host, as I have tried to make this work through VMware fusion player only and it faild !, and I think because i have an apple silicon device.  
Or you can use SSH instead of XRDP

**NOTE**    You don't have to actually enable `xrdp` or `ssh` to enable copy and paste, as I have descovered later that copy paste from/to VM, will work fine after you convert your Ubunutu server to desktop !!!

### Guide on how to use `xrdp`  
***NOTE***  Unforuantly xrdp didn't want to work on Ubunut 20.04 LTS
1-  Install some necessary packages to make remote desktop work `sudo apt install -y vim net-tools openssh-server xrdp`

2-  This will enable xrdp on ubuntu `sudo systemctl enable --now xrdp`  

3-  To make things easier, lets assign a static IP to our Ubuntu 
```bash
# This to know your network interface
# It will be somthing like this "ens160"
ip link
cd /etc/netplan  
sudo cp 00-installer-config.yaml netplan1.yaml
sudo vim netplan1.yaml
```  
Then add the following configuration  
```YAML
# This is the network config written by 'subiquity'
network:
  version: 2
  renderer: networkd # Add this if you are using the server edition and not the desktop edition of ubuntu
  ethernets:
    ens160:
      dhcp4: false
      addresses: [192.168.1.50/24] # The desired static ip address
      gateway4: 192.168.1.1 # Default gatway
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4] # DNS server address
```

```bash
sudo netplan generate netplan1.yaml # This will show no output if the file is written correctly
sudo netplan apply netplan1.yaml # This will apply the new network configuration

if config # Check that the ip address has changed !
```  

3- Also you need to change the network of the vm (VMware fusion player):
    1.  Shutdown the vm
    2.  Open vm settings
    3.  Go to *Network Adapter*
    4.  Then switch to *Wi-Fi*

4-  Download *xrdp* from app store on mac

5-  Now lets connect using the *xrdp*, open *xrdp* app  mac and then click on the plus sign

6-  Then click on `Add PC`

6- Type in the host name or your static ip address of the ubuntu VM  device  in `PC name` field then click `add`

7-  Then click the window of the new device you just added, then connect to it using your username and password

### Guide on how to use `ssh`  

1.  Install some necessary packages to make remote desktop work `sudo apt install -y vim net-tools openssh-server`

3.  To make things easier, lets assign a static IP to our Ubuntu
```bash
# This to know your network interface
# It will be somthing like this "ens160"
ip link
cd /etc/netplan  
sudo cp 00-installer-config.yaml netplan1.yaml
sudo vim netplan1.yaml
```  

Then add the following configuration  
```YAML
# This is the network config written by 'subiquity'
network:
  version: 2
  renderer: networkd # Add this if you are using the server edition and not the desktop edition of ubuntu
  ethernets:
    ens160:
      dhcp4: false
      addresses: [192.168.1.50/24] # The desired static ip address
      gateway4: 192.168.1.1 # Default gatway
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4] # DNS server address
```

```bash
sudo netplan generate netplan1.yaml # This will show no output if the file is written correctly
sudo netplan apply netplan1.yaml # This will apply the new network configuration

if config # Check that the ip address has changed !
```  

3. Also you need to change the network of the vm (VMware fusion player):
    1.  Shutdown the vm
    2.  Open vm settings
    3.  Go to *Network Adapter*
    4.  Then switch to *Wi-Fi*

4.  Open up *terminal* of mac

5. Execute this: `ssh -Y UsernameOfUbuntu@hostname/ip`

6. Then type the password

7. Now you are controlling the remote VM

# VM setup on Windows

## Creating Ubuntu image and setting up VM using Hyber-V
-   Search for "Hyber-V" using windows search bar
-   Enable "Hyber-V" checkbox then press ok
-   Restart your PC
-   Search for "Hyber-V" using windows search bar again
-   Click on "Hyper-V Quick Create"
-   Choose Ubuntu 20.04 LTS
-   Then press on Create Virtual Machine
-   Wait and... DONE !! Now hopfully Your Ubuntu has been installed successfully!

***NOTE***  If you can't find it then do this:

-   Search for "turn windows features on or off" in the windows search bar.
-   Select and enable "Hyper-V"
-   Restart your device!

***NOTE***  Follow steps in section "Configuring Ubuntu and VM to make...." if only `Enhanced session mode` is not working in your VM

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

What to do if you encounter this Error: You have encounterd a black screen

-   Then that means that your are already logged in!, so you need to logout first!
-   To logout, start ubuntu image using Basic Session from the top bar of Hyber-V window, then login, then open terminal and then logout using sudo pkill -KILL -u <username> or gnome-session-quit
-   Then go back to "Enhanced Session" and login again!
    
# Assigning the correct RAM memory, Local storage to your VM
***NOTE***  THis a necessaty step as the space of Hadoop and Ranger will take more than the default allocated space at

### RAM

#### Using Hyper-v
1.  Right click on your VM in hyper-V manger, then press "settings"
2.  Choose "Memory" then write "4096" in "RAM" field to make your VM take 4GB of mememory
3.  Click Apply then OK  

#### Using Vmware
-   Very easy...

### Storage
***NOTE***  When you use Ubuntu server edition you actaully get the chance to expland the storage in the setup menue of the OS itself, so you can skip the following steps of.

1.  You need to shutdown your VM first and after that wait for a couple of seconds if it is "Merging"
2.  Right click on your VM in found in Hyper-V manager
3.  Then choose "Settings"
4.  Click on "Hard Drive" then Choose "Edit"
5.  Click next
6.  Then choose "Expand" then "next"
7.  Assign the appropriate storage to suit your needs, lets say 40GB
8.  Click "Next"
9.  Click "OK"
10.  Then clicl on "Apply"

&npbs;

-   *Now we have also to expand our dirve inside the guest machine to take this extra space*
12.  Start your VM
13.  Then open terminal and then type: `sudo apt-get install cloud-utils # this will install a tool to enable us to resize our disk`
14.  Identify the current Ubuntu partition (usually sda1): `sudo fdisk -l`
15.  Resize the partition (Replace sda1 with the correct partition, look for partiotion with type "Linux Filesystem"): `sudo growpart /dev/<partintionName> <partitionNumber>`  

*We need to first check if the partition is in ***LVM*** (Logical Volume Group), before we can a expand the ***filesystem***.* 

16. Check if the partion is a member of logical group or not first: `lsblk -f` see under `FSTYPE`
    A.  Not a member of any LVMs
        1.  Resize the fileSystem: `sudo resize2fs /dev/sda1`
        2.  Verify the expanded space: `df -h`
    B.  A memeber of a LVM  
        1.  We need first to extend the physical volume first, but to extend it we have to know first the <vg> and <lv>, so do this:
        ```bash
        # This will show all the info of the LVMs in your system
        # Look at "LV PATH"  
        sudo lvdisplay
        ```
        2.  Extend the physical volume: `sudo lvextend -l +100%FREE /dev/<myvg>/<mylv>`
        3.  Extend the logical volume (filesystem): `sudo resize2fs /dev/<myvg>/<myvl>`
        4.  Verify the expanded space: `df -h` *OR* `sudo lvs` *OR* `sudo fdisk -l` *OR* `lsblk -f`

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
-   Copy the public key to "authorized_keys" file `cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys`
-   Change some permissions:  
``` bash
chmod 600 ~/.ssh/authorized_keys
chmod 700 ~/.ssh  

ssh localhost # This will just check if ssh is correctly installed and configured
```  

##  Download and install Hadoop
-   First switch to 'tariq' super user `su - tariq`
-   Download hadoop:
 ```
 # This is for Windows hosts
 wget -P ~/Downloads https://downloads.apache.org/hadoop/common/hadoop-3.3.6/hadoop-3.3.6.tar.gz

 # This is for Apple silicon Mac devices (arm64)
 wget -P ~/Downloads https://dlcdn.apache.org/hadoop/common/hadoop-3.3.6/hadoop-3.3.6-aarch64.tar.gz
```
-   Finish the exctraction and insy running the following commands: 
```   bash
sudo tar xzf ~/Downloads/hadoop-3.3.6.tar.gz -C ~/Downloads
sudo mv ~/Downloads/hadoop-3.3.6 /usr/local/hadoop # This will change the folder name from hadoop-3.3.6 to hadoop
sudo chown -R hdoop:hdoop /usr/local/hadoop
```   

## Configure Hadoop Environment   
-   First you need to switch to hdoop usr `su - hdoop`

-   Run this command to set the "JAVA_HOME" env variable: `echo 'export JAVA_HOME=$(readlink -f /usr/bin/java | sed "s:bin/java::")' | tee -a /usr/local/hadoop/etc/hadoop/hadoop-env.sh`  

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
        <value>file:/home/hdoop/hdfs/namenode</value>
    </property>

    <property>
        <name>dfs.datanode.data.dir</name>
        <value>file:/home/hdoop/hdfs/datanode</value>
    </property>

    <property>
        <name>dfs.secondary.http.address</name>
        <value>localhost:50090</value>
        <description>Secondary NameNode Hostname</description>
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
    su - tariq # Switch first to tariq
    sudo mkdir -p /data/hdfs/tmp/mapred/local
    sudo mkdir -p /home/hdoop/hdfs/namenode
    sudo mkdir -p /home/hdoop/hdfs/datanode  

    # And then change there ownership to the "hdoop" user we created above  
    sudo chown -R hdoop:hdoop /data/hdfs/tmp
    sudo chown -R hdoop:hdoop /home/hdoop/hdfs
    sudo chown -R hdoop:hdoop /data/hdfs/tmp/mapred/
    sudo chown -R hdoop:hdoop /data/hdfs/tmp/mapred/local
```

## Format HDFS  
```bash
su - hdoop

# This sets the java home permenently, in bashrc file of user "hdoop"
echo 'export JAVA_HOME=$(readlink -f `which java` | sed "s:/bin/java::")' >> ~/.bashrc
source ~/.bashrc 

/usr/local/hadoop/bin/hdfs namenode -format
```

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
