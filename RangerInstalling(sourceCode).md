# Installing And Configuring *Ranger*  

## Some Prerequistes before we actually install *Ranger*  

### Installing Maven

You need first to switch to root user or *sudo* them  

``` bash
su - tariq # This my username in sudo group
cd ~/Downloads

# Download maven latest distribution tar from apache maven site
wget https://dlcdn.apache.org/maven/maven-3/3.9.6/binaries/apache-maven-3.9.6-bin.tar.gz

# Unzip the file
tar -xvf apache-maven-3.9.6-bin.tar.gz -C /usr/local # This is going to *tar* the file into /usr/local

cd /usr/local

# Set some env variables
export M2_HOME=/usr/local/apache-maven-3.9.6
export M2=$M2_HOME/bin
export PATH=$M2:$PATH

# Just to make the changes permenant
echo 'export PATH=$M2:$PATH' >> ~/.bashrc
source ~/.bashrc 
  
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
git clone https://github.com/apache/incubator-ranger.git  
cd incubator-ranger  
git checkout ranger-0.5  
```

2-  Add some evn variables to `.bashrc`  
``` bash    
export MAVEN_OPTS="-Xmx512M"
export JAVA_HOME=$(readlink -f `which java` | sed "s:/bin/java::")
export PATH=$JAVA_HOME/bin:$PATH

# This will make the JAVA_HOME and PATH permenant ! (because ~/.bashrc runs on every boot)
echo 'export JAVA_HOME=$(readlink -f `which java` | sed "s:/bin/java::")' >> ~/.bashrc
echo 'export PATH=$PATH:$JAVA_HOME/bin' >> ~/.bashrc
```

3-  Build the source code
``` bash    
cd ~/dev/incubator-ranger
export MAVEN_OPTS="-Xmx512M" 
mvn clean compile package assembly:assembly install
```  


&nbsp;
&nbsp;  
&nbsp;  
  

# ***FAILD HERE!!!***
# Potentional reasons:
  - JAVA (JDK) version is not compatible with this source code  
  