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
export M2_HOME=/usr/local/apache-­maven-­<Version>
export M2=$M2_HOME/bin
export PATH=$M2:$PATH
  
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

```
