

**File Operations**
- `ls`: List files in the current directory
- `cd [directory]`: Change the current directory to `[directory]`
- `pwd`: Print the current directory
- `cp [source] [destination]`: Copy files from `[source]` to `[destination]`
- `mv [source] [destination]`: Move files from `[source]` to `[destination]`
- `rm [file]`: Remove a file

**Process Management**
- `ps`: Display your currently active processes
- `top`: Display all running processes
- `kill [pid]`: Kill process with id `[pid]`
- `bg`: Lists stopped or background jobs; resume a stopped job in the background
- `fg`: Brings the most recent job to foreground

**File Permissions**
- `chmod [permissions] [file]`: Change the permissions (read, write, execute) for `[file]`
- `chown [user]:[group] [file]`: Change the owner and group owner for `[file]`

**Networking**
- `ping [host]`: Send an ICMP echo request to `[host]`
- `ifconfig`: Display the current network configuration
- `netstat`: Networking statistics
- `ssh [user]@[host]`: Connect to `[host]` as `[user]`

**Searching**
- `grep [pattern] [files]`: Search for `[pattern]` in `[files]`
- `find /dir/ -name [file]`: Find files named `[file]` in directory `/dir/`

**System Information**
- `date`: Show the current date and time
- `uptime`: Show current uptime
- `whoami`: Who you are logged in as
- `uname -a`: Show kernel information
- `df`: Show disk usage
- `du`: Show directory space usage
- `free`: Show memory and swap usage  


&nbsp;  

---
---
---  

&nbsp;


**Colorful Output (ANSI Escape Sequences)**:
   - Use ANSI escape sequences to display colored text:
     ```
     echo -e "\\033[1;31mHello, World!\\033[0m"
     ```
   - This will print "Hello, World!" in red.

**Timestamps**:
   - Create a timestamp using:
     ```
     echo "Current date and time: $(date)"
     ```

**List Users**:
   - To extract a list of users on your system:
     ```
     cut -d: -f1 /etc/passwd
     ```

**Check Software Version**:
   - To see the version of a specific package (e.g., `firefox`):
     ```
     apt show firefox
     ```

**Disable/Enable GUI on Boot**:
   - Disable GUI (graphical interface) on boot:
     ```
     sudo systemctl set-default multi-user.target
     ```
   - Enable GUI on boot:
     ```
     sudo systemctl set-default graphical.target
     ```

**Remove GNOME Dock**:
   - If you're using GNOME, remove the dock panel:
     ```
     gnome-extensions disable ubuntu-dock@ubuntu.com
     ```

**List and Delete PPA Repositories**:
   - List PPA repositories:
     ```
     ls /etc/apt/sources.list.d/
     ```
   - Delete a specific PPA repository:
     ```
     sudo rm /etc/apt/sources.list.d/repository-name.list
     ```

**Disable Automatic Updates**:  
-   To disable automatic updates:  

    ``` bash
      sudo systemctl stop apt-daily.timer  
      sudo systemctl disable apt-daily.timer
    ```

**Disk commands**:  
``` bash
df -l # Shows all partitions and disks
sudo cfdisk # Shows hard drives and enable to expand any of them
sudo fdisk -l # Identify the current Ubuntu partition (usually sda2)

```

**How to remova a package completely from your system**  
Letts say for examle we want to remove maven package compleletey !
``` bash    
sudo apt-get remove maven
sudo apt-get remove --auto-remove maven
sudo apt-get purge maven
rm -rf ~/.m2


sudo apt-get remove openjdk-8-jdk;sudo apt-get remove --auto-remove openjdk-8-jdk;sudo apt-get purge openjdk-8-jdk

sudo apt-get remove openjdk-11-jdk;sudo apt-get remove --auto-remove openjdk-11-jdk;sudo apt-get purge openjdk-11-jdk

openjdk-11-jdk

```  

**To list all the files installed by a package use**  

```bash   
dpkg -L openjdk-8-jdk
```  

**To install two versions of java and switch between them**  
```bash    
sudo apt install openjdk-8-jdk openjdk-8-jre
sudo update-alternatives --config java  
# Then choose the suitabel version for you  

export JAVA_HOME=<path to java home of the choosen one>
```

**To delete a user**  

```bash
sudo killall -u solr # Kill all the provessess started by the user first  
sudo userdel solr  
```

/opt/solr/ranger_audit_server/scripts/start_solr.sh
