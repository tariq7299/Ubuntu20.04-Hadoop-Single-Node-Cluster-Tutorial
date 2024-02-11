-   How to correctly configure hdfs-sit.xml
-   How to stop all hadoop process
-   




```
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
chmod 700 ~/.ssh
```

[INFO] HBase Security Plugin .............................. SUCCESS [01:54 min]
[INFO] Hdfs Security Plugin ............................... SUCCESS [01:24 min]
[INFO] Hive Security Plugin ............................... SUCCESS [06:02 min]
[INFO] Knox Security Plugin Shim .......................... SUCCESS [ 49.831 s]
[INFO] Knox Security Plugin ............................... FAILURE [05:15 min]
[INFO] Storm Security Plugin .............................. SKIPPED
[INFO] YARN Security Plugin ............................... SKIPPED
[INFO] Ozone Security Plugin .............................. SKIPPED
[INFO] Ranger Util ........................................ SKIPPED
[INFO] Unix Authentication Client ......................... SKIPPED
[INFO] User Group Synchronizer Util ....................... SKIPPED
[INFO] ranger-authn ....................................... SKIPPED
[INFO] Security Admin Web Application ..................... SKIPPED
[INFO] KAFKA Security Plugin .............................. SKIPPED
[INFO] SOLR Security Plugin ............................... SKIPPED
[INFO] NestedStructure Security Plugin .................... SKIPPED
[INFO] NiFi Security Plugin ............................... SKIPPED
[INFO] NiFi Registry Security Plugin ...................... SKIPPED
[INFO] Presto Security Plugin ............................. SKIPPED
[INFO] Kudu Security Plugin ............................... SKIPPED
[INFO] Unix User Group Synchronizer ....................... SKIPPED
[INFO] Ldap Config Check Tool ............................. SKIPPED
[INFO] Unix Authentication Service ........................ SKIPPED
[INFO] KMS Security Plugin ................................ SKIPPED
[INFO] Tag Synchronizer ................................... SKIPPED
[INFO] Hdfs Security Plugin Shim .......................... SKIPPED
[INFO] Hive Security Plugin Shim .......................... SKIPPED
[INFO] YARN Security Plugin Shim .......................... SKIPPED
[INFO] OZONE Security Plugin Shim ......................... SKIPPED
[INFO] Storm Security Plugin shim ......................... SKIPPED
[INFO] KAFKA Security Plugin Shim ......................... SKIPPED
[INFO] SOLR Security Plugin Shim .......................... SKIPPED
[INFO] Atlas Security Plugin Shim ......................... SKIPPED
[INFO] KMS Security Plugin Shim ........................... SKIPPED
[INFO] Presto Security Plugin Shim ........................ SKIPPED
[INFO] ranger-examples .................................... SKIPPED
[INFO] Ranger Examples - Conditions and ContextEnrichers .. SKIPPED
[INFO] Ranger Examples - SampleApp ........................ SKIPPED
[INFO] Ranger Examples - Ranger Plugin for SampleApp ...... SKIPPED
[INFO] sample-client ...................................... SKIPPED
[INFO] Apache Ranger Examples Distribution ................ SKIPPED
[INFO] Ranger Tools ....................................... SKIPPED
[INFO] Atlas Security Plugin .............................. SKIPPED
[INFO] SchemaRegistry Security Plugin ..................... SKIPPED
[INFO] Sqoop Security Plugin .............................. SKIPPED
[INFO] Sqoop Security Plugin Shim ......................... SKIPPED
[INFO] Kylin Security Plugin .............................. SKIPPED
[INFO] Kylin Security Plugin Shim ......................... SKIPPED
[INFO] Elasticsearch Security Plugin Shim ................. SKIPPED
[INFO] Elasticsearch Security Plugin ...................... SKIPPED
[INFO] Apache Ranger Distribution ......................... SKIPPED
[INFO] Trino Security Plugin .............................. SKIPPED
[INFO] Unix Native Authenticator .......................... SKIPPED
[INFO] Trino Security Plugin Shim ......................... SKIPPED
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  37:57 min
[INFO] Finished at: 2024-02-11T14:58:34+02:00
[INFO] ------------------------------------------------------------------------
[ERROR] Failed to execute goal org.apache.maven.plugins:maven-surefire-plugin:3.0.0-M6:test (default-test) on project ranger-knox-plugin: 
[ERROR] 
[ERROR] Please refer to /home/tariq/dev/ranger/knox-agent/target/surefire-reports for the individual test results.
[ERROR] Please refer to dump files (if any exist) [date].dump, [date]-jvmRun[N].dump and [date].dumpstream.
[ERROR] -> [Help 1]
[ERROR] 
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR] 
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/MojoFailureException
[ERROR] 
[ERROR] After correcting the problems, you can resume the build with the command
[ERROR]   mvn <args> -rf :ranger-knox-plugin


