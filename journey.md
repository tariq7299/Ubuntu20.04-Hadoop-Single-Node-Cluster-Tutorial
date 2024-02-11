# This my journey  

- I found out that i must increase the storage of the VM than the default space because it is not enough  
- Then when i tried to expand the drive is said i should get rid of all the checkpoints first  
- When i tried to merge the checkpoints into my vhdx, i accedentily corrupted the vhdx then i had to create a completely new one and followed the tutorials once more
- Created backups also !
- Docker tutorial faild !, and is seems like the problem is somthing inside source code itself !, because it was like 3 steps tutorial
*THEN*  
- Went to a whole new tutorial that i will build Ranger using the source code itself and not as a pre build container
- It faild at step ***Build the source code*** after I try to run `mvn clean compile package assembly:assembly install` command, so suspect that the error is with the tutrial itself
*THEN*  
- I went to another tutorial, and it outputs the same error again!  
*THEN*  
- I suspected that those maybe are all old tutorial and they are maybe obsulete!, so i went to the official website of *Apache Ranger* and then followed the steps there and.....it faild again at the `mvn` command !  
*THEN*
- Then i suspected that maybe the preblem is with the JAVA version, because in the first tutorial is said *"Make sure you have java 7 installed"*, remove my current java (java 11) and instead installed java 7
- After that I went to the tutorial fo the source code (second attempt) [Tutorial-2](https://cwiki.apache.org/confluence/display/RANGER/Apache+Ranger+0.5.0+Installation) tried to run `mvn` command again 
- And it faild !!!!,
- And the error said that the maven versison I currently own (latest version) is not compatible with java 7 !!!!!, In spite of the tutorial itself said download the latest version of *MAVEN* !@@!@!@!@1??!?!?!  
- So i instead removed it and then downloaded a compatible version with java 7
- Then i tried `mvn clean compile package assembly:assembly install` and ...... it FAILD AGAIN!!!!  
- So i start to look in the error messages that appeard and do some investegation, and i found out that one of the dependecies *HBASE* is the one producing this error !, because when mnaven try to download this depedncy from the internet, it does not find it !, becasue the *URL* provided of *HBase* version 1.1.0 is actually corrupted and shows *404 not found* !! 
- So now I tried to download HBase dependecy manually and then edit the `pom.xml` file, of the ranger source code, and tell maven that the hbase dependecey is acually located localy on my device  
- But this method also faild!  
*THEN*
- Then i tried tuorial-4 [ApacheRangerofficialPage](https://ranger.apache.org/quick_start_guide.html) agian and i succedded with `sudo` and then it showed a new error !!!!, the error is about a faild test regarding `Knox`    
-   So I tried to turn of tests `sudo mvn -Pall -DskipTests=true clean compile package install` it faild !
*THEN*  
-   I tried to upgrade maven to the latest version and then tried this `sudo /usr/local/maven/apache-maven-3.9.6/bin/mvn -Pall -DskipTests=true -Dspotbugs.skip=true -Dchkstyle.skip=true clean compile package install `  
-   And FAILD!!!
-   And now it show this error `ppeard  
- [ERROR] Failed to execute goal org.apache.maven.plugins:maven-assembly-plugin:2.6:single (default) on project ranger-distro: Failed to create assembly: Artifact: org.apache.ranger:ranger-trino-plugin:jar:3.0.0-SNAPSHOT (included by module) does not have an artifact with a file. Please ensure the package phase is run before the assembly is generated.` ????!?!?!?!  
....
 