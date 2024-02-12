# Installing And Configuring *Ranger* from [ApacheRangerLatestVersion-OfficialApacheWebsite](https://ranger.apache.org/quick_start_guide.html)

## Some Prerequistes before we actually install *Ranger*  

### Installing Maven

You need first to switch to root user or *sudo* them : `su - tariq # This my username in sudo group`  

*Note*: This is the only way to install the latest version of maven, the upcoming other way of using `sudo apt install maven` doesn't   

``` bash
# Download maven latest distribution tar from apache maven site 
# This the appropriate version of maven that is suitble for JDK/JAVA 7
wget -P ~/Downloads https://dlcdn.apache.org/maven/maven-3/3.9.6/binaries/apache-maven-3.9.6-bin.tar.gz

# Unzip the file
sudo tar -xvf ~/Downloads/apache-maven-3.9.6-bin.tar.gz -C /usr/local # This is going to *tar* the file into /usr/local

# Set some env variables
export M2_HOME=/usr/local/apache-maven-3.9.6
export M2=$M2_HOME/bin
export PATH=$M2:$PATH

# Just to make the changes permenant
echo 'export M2_HOME=/usr/local/apache-maven-3.9.6' >> ~/.bashrc
echo 'export M2=$M2_HOME/bin' >> ~/.bashrc
echo 'export PATH=$M2:$PATH' >> ~/.bashrc
source ~/.bashrc 
```      

***OR***  

*Note*: This does'not install the latest version of maven (But it doesn't matter actaully, it will also work)
``` bash   
sudo apt update
sudo apt install maven 
```    

``` bash    
# Now to test your install of Maven, enter the following command
mvn -version
```

### Install GIT  
``` bash  
sudo apt install git
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
Ranger is still an incubater porject !, so that means there not yet compiled version of it (.exe), so we have to build Ranger using the source code!  
``` bash  
mkdir ~/dev  
cd ~/dev  
git clone https://github.com/apache/range
cd ranger  
````

2-  Add some evn variables to `.bashrc`  
``` bash    
export JAVA_HOME=$(readlink -f `which java` | sed "s:/bin/java::")
export PATH=$JAVA_HOME/bin:$PATH

# This will make the JAVA_HOME and PATH permenant ! (because ~/.bashrc runs on every boot)
echo 'export JAVA_HOME=$(readlink -f `which java` | sed "s:/bin/java::")' >> ~/.bashrc
echo 'export PATH=$PATH:$JAVA_HOME/bin' >> ~/.bashrc
```

3-  Build the source code
``` bash
cd ~/dev/ranger
```  

``` bash  
# IF you used "sudo apt install maven"
mvn -Pall clean
mvn -Pall -DskipTests=true -Dspotbugs.skip=true -Dchkstyle.skip=true clean compile package install
```  
***OR***

``` bash    

# IF you manully installed the latest version"
# I here used the full path on "mvn" .exe because it didn't work otherwise
sudo /usr/local/maven/apache-maven-3.9.6/bin/mvn -Pall clean 
sudo /usr/local/maven/apache-maven-3.9.6/bin/mvn -Pall -DskipTests=true -Dspotbugs.skip=true -Dchkstyle.skip=true clean compile package install 
```    

&nbsp;
&nbsp;  
&nbsp;  
    

# ***FAILD HERE!!!***  
## Error Appeard  
- [ERROR] Failed to execute goal org.apache.maven.plugins:maven-assembly-plugin:2.6:single (default) on project ranger-distro: Failed to create assembly: Artifact: org.apache.ranger:ranger-trino-plugin:jar:3.0.0-SNAPSHOT (included by module) does not have an artifact with a file. Please ensure the package phase is run before the assembly is generated. -> [Help 1]  

## Potentional reasons:
  - ?

