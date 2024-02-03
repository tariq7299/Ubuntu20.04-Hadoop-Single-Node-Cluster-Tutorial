# Hadoop Cheatsheet

### Hadoop File System (HDFS) Commands:

1. **List files in HDFS:**
   ```bash
   hdfs dfs -ls /path/to/directory
   ```

2. **Create a directory in HDFS:**
   ```bash
   hdfs dfs -mkdir /path/to/new_directory
   ```

3. **Copy from local file system to HDFS:**
   ```bash
   hdfs dfs -copyFromLocal /path/to/local/file /path/in/hdfs
   ```

4. **Copy from HDFS to local file system:**
   ```bash
   hdfs dfs -copyToLocal /path/in/hdfs /path/to/local/directory
   ```

5. **Read the content of a file in HDFS:**
   ```bash
   hdfs dfs -cat /path/to/file
   ```

6. **Remove a file or directory in HDFS:**
   ```bash
   hdfs dfs -rm /path/to/file_or_directory
   ```

7. **Check disk usage in HDFS:**
   ```bash
   hdfs dfs -du -h /path/to/directory
   ```

### MapReduce Commands:

1. **Run a MapReduce job:**
   ```bash
   hadoop jar /path/to/your/mapreduce.jar input_directory output_directory
   ```

2. **View job status:**
   ```bash
   yarn application -list
   ```

3. **Kill a running MapReduce job:**
   ```bash
   yarn application -kill <application_id>
   ```

### Hadoop Cluster Management:

1. **Start Hadoop services:**
   ```bash
   start-all.sh
   ```

2. **Stop Hadoop services:**
   ```bash
   stop-all.sh
   ```

3. **Start a specific service (e.g., HDFS):**
   ```bash

   /usr/local/hadoop/sbin/start-dfs.sh # Start NameNode, DataNode, Secondary NameNode, 
   /usr/local/hadoop/sbin/start-yarn.sh # Start ResourceManager, NodeManager
   ```

4. **Stop a specific service (e.g., HDFS):**
   ```bash
    /usr/local/hadoop/sbin/stop-dfs.sh
   ```

### Configuration Files:

1. **Edit Hadoop configurations:**
   ```bash
   nano $HADOOP_HOME/etc/hadoop/hdfs-site.xml
   ```

2. **Reload Hadoop configurations:**
   ```bash
   source ~/.bashrc
   ```

3. **Check Hadoop environment variables:**
   ```bash
   echo $HADOOP_HOME
   ```

### Miscellaneous:

1. **Check Hadoop version:**
   ```bash
   hadoop version
   ```

2. **Check HDFS health:**
   ```bash
   hdfs dfsadmin -report
   ```

3. **Format the HDFS NameNode:**
   ```bash
   hdfs namenode -format
   ```
3. **Print namenode logs**
   ```bash
   cat /usr/local/hadoop/logs/hadoop-hdoop-namenode-tariq-Virtual-Machine.log
   ```
3. **Format the HDFS NameNode:**
   ```bash
   hdfs namenode -format
   ```
