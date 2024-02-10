# Installing And Configuring *Ranger*  

## Some Prerequistes before we actually install *Ranger*  

### Installing Maven

You need first to switch to root user or *sudo* them  

```
su - tariq # This my username in sudo group
cd ~/Downloads

# Download maven latest distribution tar from apache maven site
wget https://dlcdn.apache.org/maven/maven-3/3.9.6/binaries/apache-maven-3.9.6-bin.tar.gz

# Unzip the file
tar -xvf apache-maven-3.9.6-bin.tar.gz -C /usr/local #This is going to *tar* the file into /usr/local

cd /usr/local

# Set some env variables
export M2_HOME=/usr/local/apache-­maven-­<Version>
export M2=$M2_HOME/bin
export PATH=$M2:$PATH
  
#Now to test your install of Maven, enter the following command
mvn -version
```
