### Install Oracle java and uppdate JAVA_HOME (jdk)

Create a directory for oracle:

	# mkdir -p /opt/oracle
    
Download latest Java SE from Oracle to this directory. Extract.

	# tar xzvf jdk-8u60-linux-x64.tar.gz
        
View and add this location to Debian system with:

	# update-alternatives --list java
	/usr/lib/jvm/java-7-openjdk-amd64/jre/bin/java
    
Trying to configure only one group:

	# update-alternatives --config java
	There is only one alternative in link group java (providing 		/usr/bin/java): /usr/lib/jvm/java-7-openjdk-amd64/jre/bin/java
	Nothing to configure.

	# update-alternatives --install /opt/oracle_java8 oracle_java8 		/opt/oracle/jdk1.8.0_60/bin/java 2
	update-alternatives: using /opt/oracle/jdk1.8.0_60/bin/java to 		provide /opt/oracle_java8 (oracle_java8) in auto mode

Display current version:

	# java -version
	java version "1.7.0_79"
	OpenJDK Runtime Environment (IcedTea 2.5.6) (7u79-2.5.6-1~deb8u1)
	OpenJDK 64-Bit Server VM (build 24.79-b02, mixed mode)

Add new alternative (we cannot use /usr/bin/java direct) - in use!:

	# update-alternatives --install /opt/java java 						/opt/oracle/jdk1.8.0_60/bin/java 2
    
	# update-alternatives --list java
	/opt/oracle/jdk1.8.0_60/bin/java
	/usr/lib/jvm/java-7-openjdk-amd64/jre/bin/java
    
Now configure oracle java8

	# update-alternatives --config java
	There are 2 choices for the alternative java (providing 			/opt/java).

  	Selection    Path                                            		Priority   Status
	------------------------------------------------------------
	* 0            /usr/lib/jvm/java-7-openjdk-amd64/jre/bin/java   	1071      auto mode
  	1            /opt/oracle/jdk1.8.0_60/bin/java                 2         manual mode
  	2            /usr/lib/jvm/java-7-openjdk-amd64/jre/bin/java   	1071      manual mode

Now select *1*:

	update-alternatives: using /opt/oracle/jdk1.8.0_60/bin/java to 		provide /opt/java (java) in manual mode

Now to have the system find this for java run (now /usr/bin/java is not in use):

	# update-alternatives --install /usr/bin/java java 					/opt/oracle/jdk1.8.0_60/bin/java 2
	update-alternatives: renaming java link from /opt/java to 			/usr/bin/java
    
	# which java
	/usr/bin/java
	# java -version
	java version "1.8.0_60"
	Java(TM) SE Runtime Environment (build 1.8.0_60-b27)
	Java HotSpot(TM) 64-Bit Server VM (build 25.60-b23, mixed mode)
