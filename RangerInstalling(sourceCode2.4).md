# Installing And Configuring *Ranger* from [CERN Summer student Program 2023](https://cds.cern.ch/record/2872121/files/CERN%20Report%20Cl%C3%A9ment%20LUCAS.pdf)  

## Some Prerequistes before we actually install *Ranger*    

### Installing java 11 (openJDK 11)  

-   Make sure you have java 11 installed  
```bash   
tariq@tariq-Virtual-Machine:~/git/dev/ranger$ java -version
openjdk version "11.0.21" 2023-10-17
OpenJDK Runtime Environment (build 11.0.21+9-post-Ubuntu-0ubuntu120.04)
OpenJDK 64-Bit Server VM (build 11.0.21+9-post-Ubuntu-0ubuntu120.04, mixed mode, sharing)
```  
If it is not installed then run this:   

```bash   
sudo apt install openjdk-11-jdk -y  
export JAVA_HOME=/usr/lib/jvm/java-1.11.0-openjdk-amd64  
```  

*Note*: If you have another version installed already and you don't want to remove it do this:  

```bash 
sudo apt install openjdk-11-jdk -y  
sudo update-alternatives --config java
# Then choose the java 11 to be the current java

# Set JAVA_HOME  
export JAVA_HOME=/usr/lib/jvm/java-1.11.0-openjdk-amd64 
``` 



### Installing Maven

You need first to switch to root user or *sudo* them : `su - tariq # This my username in sudo group`  

*Note*: This is the only way to install the latest version of maven, the upcoming other way of using `sudo apt install maven` doesn't   

``` bash  

sudo mkdir /usr/local/maven  

# Download maven latest distribution tar from apache maven site 
# This the appropriate version of maven that is suitble for JDK/JAVA 7
wget -P ~/Downloads https://dlcdn.apache.org/maven/maven-3/3.9.6/binaries/apache-maven-3.9.6-bin.tar.gz

# Unzip the file
sudo tar -xvf ~/Downloads/apache-maven-3.9.6-bin.tar.gz -C /usr/local/maven # This is going to *tar* the file into /usr/local

# Set some env variables
export M2_HOME=/usr/local/maven/apache-maven-3.9.6
export M2=$M2_HOME/bin
export PATH=$M2:$PATH

# Just to make the changes permenant
echo 'export M2_HOME=/usr/local/maven/apache-maven-3.9.6' >> ~/.bashrc
echo 'export M2=$M2_HOME/bin' >> ~/.bashrc
echo 'export PATH=$M2:$PATH' >> ~/.bashrc
source ~/.bashrc 
```  

``` bash    
# Now to test your install of Maven, enter the following command
mvn -version
```  



### Install GIT  
``` bash  
sudo apt install git
```


### Install GCC  

```bash   
sudo apt install gcc
```  

### Install MySQL  

1-  First you need to install MySQL
``` bash
sudo apt install mysql-server  
sudo service mysql status # The MySQL service should start automaticlly verify by this command
sudo systemctl start mysql.service # If not, This will start the my sql service   
```  
2-  Configure security installation

This following will prompt you to:
  - Set a root password (if not already during installation)
  - Remove anonmous users  
  - Disallow root login remotely  
  - Remove the test database
  - Reload privilege tables      

And I choose *No* for testing purposes!
``` bash
sudo mysql_secure_installation
```  

3-  Download the MySQL JDBC, and then untarit, then finally copy the .jar file to a common location where everyone be abel to access it.

``` bash  
# This will download the appropriate MySQL JDBC packge for Ubuntu 20.04 LTS
wget -P ~/Downloads https://downloads.mysql.com/archives/get/p/3/file/mysql-connector-j_8.0.33-1ubuntu20.04_all.deb
sudo dpkg -i ~/Downloads/mysql-connector-j_8.0.33-1ubuntu20.04_all.deb # This will install the .deb package
```   

## Install And Build Ranger from the source  

1-  Clone the ranger source code  
``` bash  
mkdir ~/git/dev  
cd ~/git/dev
sudo git clone https://github.com/apache/ranger.git
cd ranger 
# This will make us build the latest version to date (Ranger 2.4)
git checkout tags/release-ranger-2.4.0 -b ranger-2.4
````

2-  Build the source code
``` bash  
# /usr/local/apache-maven-3.9.6/bin/mvn Iam using the full path to "mvn" exec BECAUSE JUST IN CASE !!!!!!!!!!!  
# "clean compile package install" This will make a clean the old files of the previuos build and then comile and after that package the files and finally install
# "-DskipTests=true -Dspotbugs.skip=true -Dchkstyle.skip=true" This will skip the Unit test and some other checks that is irrelevent to the build process
# "-Dassembly.plugin.version=3.1.0 -Dhadoop.version=3.3.0 -Dhbase.version=2.3.4 -Dzookeeper.version=3.6.1 -Dhive.version=3.0.0 -Dmysql-connector-java.version=8.0.28" This is just specifying some versions of the other Apache components
# "'!plugin-ozone, !plugin-solr, !plugin-nifi, !plugin-nifi-registry, !plugin-kudu, !plugin-kms, !ranger-ozone-plugin-shim, !storm-agent, !ranger-storm-plugin-shim, !ranger-solr-plugin-shim, !ranger-atlas-plugin-shim, !plugin-atlas, !plugin-kylin, !ranger-kylin-plugin-shim'" This disables those plugins completely, because we don't need them

sudo /usr/local/apache-maven-3.9.6/bin/mvn clean compile package install -DskipTests=true -Dspotbugs.skip=true -Dchkstyle.skip=true -Dassembly.plugin.version=3.1.0 -Dhadoop.version=3.3.0 -Dhbase.version=2.3.4 -Dzookeeper.version=3.6.1 -Dhive.version=3.0.0 -Dmysql-connector-java.version=8.0.28 -pl '!plugin-ozone, !plugin-solr, !plugin-nifi, !plugin-nifi-registry, !plugin-kudu, !plugin-kms, !ranger-ozone-plugin-shim, !storm-agent, !ranger-storm-plugin-shim, !ranger-solr-plugin-shim, !ranger-atlas-plugin-shim, !plugin-atlas, !plugin-kylin, !ranger-kylin-plugin-shim'
```  


3- Make sure the build was successfull
```bash  
# List all the tar files,
ls target/*.tar.gz
```