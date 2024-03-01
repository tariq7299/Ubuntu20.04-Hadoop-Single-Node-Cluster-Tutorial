# Installing And Configuring *Ranger* from [CERN Summer student Program 2023](https://cds.cern.ch/record/2872121/files/CERN%20Report%20Cl%C3%A9ment%20LUCAS.pdf)  x

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
export JAVA_HOME=/usr/lib/jvm/java-1.11.0-openjdk-amd64/
# And this for apple silicon devices  
export JAVA_HOME=/usr/lib/jvm/java-1.11-openjdk-arm64/
```  

*Note*: If you have another version installed already and you don't want to remove it do this:  

```bash 
sudo apt install openjdk-11-jdk -y  
sudo update-alternatives --config java
# Then choose the java 11 to be the current java

# Set JAVA_HOME  
export JAVA_HOME=/usr/lib/jvm/java-1.11.0-openjdk-amd64 
# OR this for apple silicon devices
export JAVA_HOME=/usr/lib/jvm/java-1.11.0-openjdk-arm64 
``` 



### Installing Maven

You need first to switch to root user or *sudo* them : `su - tariq # This my username in sudo group`  

*Note*: This is the only way to install the *latest version* of maven, the upcoming other way of using `sudo apt install -y maven` doesn't   

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
sudo apt install -y gcc
```  

## Install And Build Ranger from the source

1-  Clone the ranger source code  
``` bash  
mkdir -p ~/git/dev
cd ~/git/dev
sudo git clone https://github.com/apache/ranger.git
cd ranger 
# This will make us build the latest version to date (Ranger 2.4)
sudo git checkout tags/release-ranger-2.4.0 -b ranger-2.4
````

2-  Build the source code
``` bash  
# /usr/local/apache-maven-3.9.6/bin/mvn Iam using the full path to "mvn" exec BECAUSE JUST IN CASE !!!!!!!!!!!  
# "clean compile package install" This will make a clean the old files of the previuos build and then comile and after that package the files and finally install
# "-DskipTests=true -Dspotbugs.skip=true -Dchkstyle.skip=true" This will skip the Unit test and some other checks that is irrelevent to the build process
# "-Dassembly.plugin.version=3.1.0 -Dhadoop.version=3.3.0 -Dhbase.version=2.3.4 -Dzookeeper.version=3.6.1 -Dhive.version=3.0.0 -Dmysql-connector-java.version=8.0.28" This is just specifying some versions of the other Apache components
# "'!plugin-ozone, !plugin-solr, !plugin-nifi, !plugin-nifi-registry, !plugin-kudu, !plugin-kms, !ranger-ozone-plugin-shim, !storm-agent, !ranger-storm-plugin-shim, !ranger-solr-plugin-shim, !ranger-atlas-plugin-shim, !plugin-atlas, !plugin-kylin, !ranger-kylin-plugin-shim'" This disables those plugins completely, because we don't need them

sudo /usr/local/maven/apache-maven-3.9.6/bin/mvn clean compile package install -DskipTests=true -Dspotbugs.skip=true -Dchkstyle.skip=true -Dassembly.plugin.version=3.1.0 -Dhadoop.version=3.3.6 -Dhbase.version=2.3.4 -Dzookeeper.version=3.6.1 -Dhive.version=3.0.0 -Dmysql-connector-java.version=8.0.33 -pl '!plugin-ozone, !plugin-solr, !plugin-nifi, !plugin-nifi-registry, !plugin-kudu, !plugin-kms, !ranger-ozone-plugin-shim, !storm-agent, !ranger-storm-plugin-shim, !ranger-solr-plugin-shim, !ranger-atlas-plugin-shim, !plugin-atlas, !plugin-kylin, !ranger-kylin-plugin-shim'

# This with solr
sudo /usr/local/maven/apache-maven-3.9.6/bin/mvn clean compile package install -DskipTests=true -Dspotbugs.skip=true -Dchkstyle.skip=true -Dassembly.plugin.version=3.1.0 -Dhadoop.version=3.3.6 -Dhbase.version=2.3.4 -Dzookeeper.version=3.6.1 -Dhive.version=3.0.0 -Dmysql-connector-java.version=8.0.33 -pl '!plugin-ozone, !plugin-nifi, !plugin-nifi-registry, !plugin-kudu, !plugin-kms, !ranger-ozone-plugin-shim, !storm-agent, !ranger-storm-plugin-shim, !ranger-atlas-plugin-shim, !plugin-atlas, !plugin-kylin, !ranger-kylin-plugin-shim'
```  


3- Make sure the build was successfull
```bash  
# List all the tar files,
ls target/*.tar.gz
```  

## Installing and Configure MySQL   

1-  First you need to install MySQL
``` bash
sudo apt install -y mysql-server
sudo systemctl status mysql.service # The MySQL service should start automaticlly verify by this command

# If not, This will start the my sql service 
# sudo systemctl start mysql.service 
```   

2-  Configure security installation

This following will prompt you to:
  - Set a root password (if not already during installation)
  - Remove anonmous users  
  - Disallow root login remotely  
  - Remove the test database
  - Reload privilege tables      

And I choose *No* for testing purposes!,
except for removing anynomes users removal
``` bash
sudo mysql_secure_installation
```    

3- Change the password of the Mysql"root" user and the authentication method

```SQL
# log in into mysql server usin "root" user
sudo mysql -u root
# This will change the auth method from "auth_socket" to "mysql_native_password"
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '1122';
FLUSH PRIVILEGES; # Reload privileges tables
```  

**NOTE**: You can access the mysql server using the root user without typing any password, because `auth_socket` is enbled by default by "MySQL" and what it does that it recogizes that you accessed my sql using `root` user of the system (Linux OS), and it authenticate you automaticlly withou typing  password of `root` `MySQL` user.  
But In order for `Ranger` to be setup and configure appropriatly we have to change this auth method for mysql users from `auth_socket` to `mysql_native_password`  

4- Create `rangeradmin` user in mysql, and change the default auth method

```sql   
# Login to mysql and create user
sudo mysql -u root -p
# This will creat user, change the default auth method and finally set a password
CREATE USER 'rangeradmin'@'localhost' IDENTIFIED WITH mysql_native_password BY '1122';
GRANT ALL PRIVILEGES ON *.* TO 'rangeradmin'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```    

5- Create a database for ranger  
```sql
CREATE DATABASE ranger; 
```

6-  Download the MySQL JDBC, and then untarit, then finally copy the .jar file to a common location where everyone be abel to access it.

``` bash  
# This will download the appropriate MySQL JDBC packge for Ubuntu 20.04 LTS
wget -P ~/Downloads https://downloads.mysql.com/archives/get/p/3/file/mysql-connector-j_8.0.33-1ubuntu20.04_all.deb

sudo dpkg -i ~/Downloads/mysql-connector-j_8.0.33-1ubuntu20.04_all.deb # This will install the .deb package
```   


## Installing and Configure Solr  

Solr is going to store the audit logs

1-  Go to  `cd ~/git/dev/ranger/security-admin/contrib/solr_for_audit_setup`    

2- Edit the setup file of solr, and configure this variables 
```bash  
sudo vim install.properties
```

```bash
SOLR_INSTALL = true
SOLR_DOWNLOAD_URL = https://archive.apache.org/dist/solr/solr/9.5.0/solr-9.5.0.tgzz
# For apple silicon devices only versions complatible with java 8 is working
# SOLR_DOWNLOAD_URL = https://archive.apache.org/dist/lucene/solr/8.9.0/solr-8.9.0.tgz 
SOLR_INSTALL_FOLDER = /opt/solr
JAVA_HOME = /usr/lib/jvm/java-11-openjdk-amd64 
# For apple silicon devices only java 8 is compatible and working
# JAVA_HOME = /usr/lib/jvm/java-8-openjdk-arm64
SOLR_USER = solr
SOLR_RANGER_HOME = /opt/solr/ranger_audit_server
SOLR_RANGER_PORT = 6083
SOLR_DEPLOYMENT = standalone
SOLR_RANGER_DATA_FOLDER = /opt/solr/ranger_audit_server/data
SOLR_LOG_FOLDER = /var/log/solr/ranger_audits
SOLR_MAX_MEM = 2g
``` 
*Note*: I wasn't able to download the latest versiion of `Solr` becasuse (very stupid reason), the url of the latest version is `https://www.apache.org/dyn/closer.lua/solr/solr/9.5.0/solr-9.5.0.tgz?action=download` and if i wrote as a value to `SOLR_DOWNLOAD_URL`, it will bug the installation process after i execute `./setup.sh`, becasue the developers of `Ranger` and `Solr`, have created a code (I beleive it somthing like a reguler exepression) to extract the file name from the Download URL, and they suppose that the url won't end with a `?action=download` (url query), because the file name that the `.tar` file will be exctracted to will be named `solr-9.5.0.tgz?action=download` and not `solr-9.5.0.tgz`.  
So I had to use the older format of the older solr versions url `https://archive.apache.org/dist/lucene/solr/8.9.0/solr-8.9.0.tgz`.  
! so weird bug, it took from me like 1 hour to spot and fix !

***NOTE*** I found a way to download the latest version, and avoid this wird bug ! b
Just specify the file name using `-O` option 
so do this : `SOLR_DOWNLOAD_URL:-O solr-9.5.0.tgz https://www.apache.org/dyn/closer.lua/solr/solr/9.5.0/solr-9.5.0.tgz?action=download`

***NOTE*** I have found another way that i can download the latest version without doing this `-O ...`, by download from the "archive" webpage by using this link [latestSolr](https://archive.apache.org/dist/solr/solr/9.5.0/solr-9.5.0.tgz)


3- Create some necesseray dir for solr
```bash
sudo mkdir -p /opt/;sudo mkdir -p /var/log/solr/ranger_audits
```  

4- Run the `.setup.sh` file  
```bash
sudo ./setup.sh
```  

5- Set a password for `Solr` that was just created 
```bash
sudo passwd solr
# 1122
```
*Note*: This user will be used to start solr servic (this necessary becasue by default after you run `setup.sh` allsu  the folders and files that will be created will belong to `solr` user)

6- You can open  `/opt/solr/ranger_audit_server/install_notes.txt`  for instructions to start and stop `Solr`

7- Start `Solr`  
```bash 
su solr # switch to solr user

/opt/solr/ranger_audit_server/scripts/start_solr.sh
```  

8- Check for solr by accessing the web portal of `localhost:6083`

## Configure Ranger Policy Admin  

1- Untar the `ranger-2.4.0-admin` file
```bash  
cd /usr/local
# Untart the file in the same dir
sudo tar xvf ~/git/dev/ranger/target/ranger-2.4.0-admin.tar.gz -C /usr/local
# This sets a symbolic link to "ranger-2.4.0-admin" so we can access the folder "ranger-2.4.0-admin" by "ranger-admin"
sudo ln -s /usr/localranger-2.4.0-admin/ /usr/localranger-admin  
cd /usr/local/ranger-admin/
```    

2-  Open `install.properties` via sudo  

```bash  
sudo vim install.properties  
```  

3-  Change the following values  
```bash  
# Mysql root
db_root_user=root
db_root_password=1122
# DB UserId used for the XASecure schema
db_name=ranger
db_user=rangeradmin
db_password=1122
# audit log
audit_store=solr
audit_solr_urls=http://localhost:6083/solr/ranger_audits
audit_solr_user=solr
# UNIX User CONFIG
unix_user_pwd=1122

# POLICYMANAGER CONFIG
policymgr_external_url=http://localhost:6080

```    

4- Create a symbolic link to MySQL connector  
```bash  
# This is necessary because ranger admin setup depend on the existins and location of this symbolic link
sudo ln -s /usr/share/java/mysql-connector-java-8.0.33.jar /usr/share/java/mysql-connector-java.jar
```  

5- run setup.sh :   
```bash
# This sets JAVA_HOME env variable for sudo environment ! (this is necesseray)
# So use JDK 8 or JDK 11 ! I think it suppose to be 8 or heigher !
sudo JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64 ./setup.sh

# And this for apple silicon devices, only compatible version is java 8
sudo JAVA_HOME=/usr/lib/jvm/java-8-openjdk-arm64 ./setup.sh
```  

6- Create necessary dir called `/var/run/ranger`
```bash  
sudo mkdir /var/run/ranger  
sudo chown ranger:ranger /var/run/ranger  
```  

7- Set password for `ranger` user
This user was automaticcly created by ranger admin
```bash  
sudo passwd ranger # This will set a password to the newly created "ranger" user  
#1122  
su - ranger # Switch first to ranger user
# "ranger" will be used to run ranger
# This user has been created automaticcly by ranger setup
```  

8- Start `ranger`  
```bash
ranger-admin start  
```    

9- Access `Ranger` web interface: Now you can visit `localhost:6080`, this will open up the ranger admin web interface  

10- access the web portal by providing the default username and password `admin`  

Then fill in the info, like the first name and last name...etc


## Configure Ranger UserSync
1- Untar the Ranger UserSync tar file  
```bash  
cd /usr/local
sudo tar xvf ~/git/dev/ranger/target/ranger-2.4.0-usersync.tar.gz
sudo ln -s ranger-2.4.0-usersync/ ranger-usersync  # Create a symbolic link
cd ranger-usersync
```  

2- Create necessary log dir
```bash   
sudo mkdir -p /var/log/ranger/usersync
sudo chown ranger:ranger /var/log/ranger && sudo chown ranger:ranger /var/log/ranger/usersync
```  

Now we have many choices to configure Ranger usersync, here we will use two of them (`unix`, `LDAP`)

### Configure Range usersync with `unix`  
1-  Head to ranger-usersync dir: `cd /usr/local/ranger-usersync`

2-  Edit the install.properties file with appropriate unix values
```bash
POLICY_MGR_URL = http://HOST_ADDRESS:6080
SYNC_SOURCE = unix
logdir = /var/log/ranger/usersync
```

3- Install usersync by running `./setup.sh`
```bash
 This sets JAVA_HOME env variable for sudo environment ! (this is necesseray)
# So use JDK 8 or JDK 11 ! I think it suppose to be 8 or heigher !
sudo JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64 ./setup.sh

# And this for apple silicon devices
sudo JAVA_HOME=/usr/lib/jvm/java-11-openjdk-arm64 ./setup.sh
```

4- Start and stop services of usersync
```bash
./ranger­-usersync-services.sh start # Access usersync from ranger web portal at localhost:6080

./ranger­-usersync-services.sh stop
```


### Configure Range usersync with `LDAP`

First we need to setup the LDAP server

#### Guide to Setup and Configue a `LDAP` server  


1- Download `OpenLDAP` package
```bash  
sudo apt install -y slapd ldap-utils

# You will be prompted to set a password for lDAP admin
#1122
```  

2- Execute this to add some configurations
```bash   
sudo dpkg-reconfigure slapd
```  
Then You will be prompted with some questions:
- Omit OpenLDAP server configuration: No  
- DNS domain name: example.com  
- Organization name: example.com  
- Password: 1122  
- Last question: yes  

3- make sure everything is working and correctly configured  
 ```bash
sudo slapcat # This will display the configuration of LDAP
```

4-  Create file base`dn` for Users and Groups  
```bash    
sudo mkdir /usr/local/ldap && sudo touch /usr/local/ldap/basedn.ldif  
sudo vim /usr/local/basedn.ldif 
```  
Add this to the file:

```YAML
dn: ou=people,dc=example,dc=com
objectClass: organizationalUnit
ou: people

dn: ou=groups,dc=example,dc=com
objectClass: organizationalUnit
ou: groups
```

5- Add the file to LDAP by running
```bash 
cd /usr/local/ldap
ldapadd -x -D cn=admin,dc=example,dc=com -W -f basedn.ldif
```  
Now we are done!  
We have now configured the essentional configuration of our LDAP server to work!

6- Now lets create a user in LDAP, and add him to it, So first we need to create a "HASH" of the password of the user that will be added to LDAP  
```bash 
sudo slappasswd
# Enter password two times  
# {SSHA}fe6dvRX6trPhTn5ypSTMsUNRZztWJThz --> This will outputed  
# copy it !
```  

7- Create `ldapusers.ldif` and add a user config values to it.
Add a user details that actaully exists on your device   
This is the file that is going to be used to add users to LDAP 
```bash
cd /usr/local/ldap
sudo vim /usr/local/ldap/ldapusers.ldif
```

```YAML
dn: uid=computingforgeeks,ou=people,dc=example,dc=com
objectClass: inetOrgPerson
objectClass: posixAccount
objectClass: shadowAccount
cn: computingforgeeks
sn: Wiz
userPassword: {SSHA}Zn4/E5f+Ork7WZF/alrpMuHHGufC3x0k # Paste the password hash you just copied
loginShell: /bin/bash
uidNumber: 2000
gidNumber: 2000
homeDirectory: /home/computingforgeeks
```  

8- Add the file `ldapusers.ldif` to LDAP that contain the user you just created  
```bash 
cd /usr/local/ldap  
ldapadd -x -D cn=admin,dc=example,dc=com -W -f ldapusers.ldif 
```
Now we are done!!  
We have added user `hdoop` to our `LDAP`

9- Create `ldapgroups.ldif` and a group configuration values to it  
```bash  
cd /usr/local/ldap  
sudo vim ldapgroups.ldif  
```  
Add those values to `ldapgroups.ldif` file, to add a group to LDAP called `hadoop`
```YAML  
dn: cn=hadoop,ou=groups,dc=example,dc=com
objectClass: posixGroup
cn: hadoop
gidNumber: 2000
memberUid: hadoop
```

10- Add the file `ldapgroups.ldif` containg the new group we just created
```bash 
ldapadd -x -D cn=admin,dc=example,dc=com -W -f ldapgroups.ldif
```

11- Test LDAP server to see if it is working or not  
```bash 
# This hopefully should display all users and groups in LDAP, with there details
ldapsearch -x -H ldap://localhost -b "dc=example,dc=com" -D "cn=admin,dc=example,dc=com" -W
```

*OR*   

You can replace this `-H ldap://localhost` with `-H ldapi:///`  
This `ldapi:///` means that the "LDAP" server is running on the same machine or within the same network.
