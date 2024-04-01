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

# This with solr
sudo /usr/local/maven/apache-maven-3.9.6/bin/mvn clean compile package install -DskipTests=true -Dspotbugs.skip=true -Dchkstyle.skip=true -Dassembly.plugin.version=3.1.0 -Dhadoop.version=3.3.6 -Dhbase.version=2.3.4 -Dzookeeper.version=3.6.1 -Dhive.version=3.0.0 -Dmysql-connector-java.version=8.0.33 -pl '!plugin-ozone, !plugin-nifi, !plugin-nifi-registry, !plugin-kudu, !plugin-kms, !ranger-ozone-plugin-shim, !storm-agent, !ranger-storm-plugin-shim, !ranger-atlas-plugin-shim, !plugin-atlas, !plugin-kylin, !ranger-kylin-plugin-shim'


