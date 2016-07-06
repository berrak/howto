## Maven for Java projects

Visit apache-mavens website:

https://maven.apache.org/

### Installation

Download latest version and follow the installation procedure given.

	apache-maven-3.3.3-bin.tar.gz

### User configuration

Add to user *.bashrc* file:

```
# for maven build system
export PATH=$PATH:/usr/local/apache-maven/apache-maven-3.3.3/bin
export MAVEN_OPTS="-Xms256m -Xmx512m"
```	
Test the installation with:

	$ mvn --version

### Use maven templates for new projects

Create a working user directory for all maven based Java projects:

	$ mkdir ~/MAVEN
	$ cd MAVEN
	$ mvn archetype:generate

[INFO] ----------------------------------------------------------------------------
[INFO] Using following parameters for creating project from Old (1.x) Archetype: maven-archetype-quickstart:1.1
[INFO] ----------------------------------------------------------------------------
[INFO] Parameter: basedir, Value: /home/bekr/MAVEN
[INFO] Parameter: package, Value: org.debinix.mwtools
[INFO] Parameter: groupId, Value: org.debinix.mwtools
[INFO] Parameter: artifactId, Value: fmw-tools
[INFO] Parameter: packageName, Value: org.debinix.mwtools
[INFO] Parameter: version, Value: 1.0-SNAPSHOT
[INFO] project created from Old (1.x) Archetype in dir: /home/bekr/MAVEN/fmw-tools
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 06:21 min
[INFO] Finished at: 2015-06-13T11:37:16+02:00
[INFO] Final Memory: 16M/290M
[INFO] ------------------------------------------------------------------------

The top level directory is the project name, i.e. *fmw-tools*. The *test* subtree is automatically generated as the two App.java-stubs.
	
    $ tree
```
`-- fmw-tools
    |-- pom.xml
    `-- src
        |-- main
        |   `-- java
        |       `-- org
        |           `-- debinix
        |               `-- mwtools
        |                   `-- App.java
        `-- test
            `-- java
                `-- org
                    `-- debinix
                        `-- mwtools
                            `-- AppTest.java
```

The project name *fmw-tools* can be the repository name on GitHub. The idea is to have packages like *mwtools.core*, *mwtools.soa*, *mwtools.oam*, *mwtools.ofr* and *mwtools.common*.

### Build application

	$ mvn clean package

### Newly created files/directories
Below *~/MAVEN/fmw-tools* directory the *target* subtree have been created.
```
`-- target
    |-- classes
    |   `-- org
    |       `-- debinix
    |           `-- mwtools
    |               `-- App.class
    |-- fmw-tools-1.0-SNAPSHOT.jar
    |-- maven-archiver
    |   `-- pom.properties
    |-- maven-status
    |   `-- maven-compiler-plugin
    |       |-- compile
    |       |   `-- default-compile
    |       |       |-- createdFiles.lst
    |       |       `-- inputFiles.lst
    |       `-- testCompile
    |           `-- default-testCompile
    |               |-- createdFiles.lst
    |               `-- inputFiles.lst
    |-- surefire-reports
    |   |-- org.debinix.mwtools.AppTest.txt
    |   `-- TEST-org.debinix.mwtools.AppTest.xml
    `-- test-classes
        `-- org
            `-- debinix
                `-- mwtools
                    `-- AppTest.class
```


### Run application

Run the application:

	$ cd fmw-tools/target/class
    $ java org.debinix.mwtools.App
    	Hello world!
        
### Add Java files

Modify existing *App.java* to:

```
package org.debinix.mwtools;

public class App
{
    public static void main( String[] args )
    {
     	Util.printMessage( "Hello World!" );
    }
}
```
Now create a new file *Util.java* like so:

```
package org.debinix.mwtools;

public class Util
{
    public static void printMessage( String message )
    {
     	System.out.println( message );
    }
}
```

Build and run:

	$ cd ~/MAVEN/fmw-tools
    $ mvn clean compile
    $ cd target/classes/org
    $ java org.debinix.mwtools.App
    	Hello world!

### Dependencies

Add the dependencies to the *pom.xml* file. If you want to copy all needed dependencies to one place, use the copy-dependencies goal. The Maven Dependency Plugin will copy all dependencies (including transitive) to the target/dependency folder in this case. **Note** You must first create the *dependency* directory first otherwise nothing is copied (despite that text say differently).

	$ mkdir target/dependency
	$ mvn dependency:copy-dependencies

```
[INFO] Copying junit-3.8.1.jar to /home/bekr/MAVEN/fmw-tools/target/dependency/junit-3.8.1.jar
[INFO] Copying jopt-simple-4.9-beta-1.jar to /home/bekr/MAVEN/fmw-tools/target/dependency/jopt-simple-4.9-beta-1.jar
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 24.187 s
[INFO] Finished at: 2015-06-13T20:42:20+02:00
[INFO] Final Memory: 15M/331M
[INFO] ------------------------------------------------------------------------
```
Clean, compile and package:

	$ mvn clean
    $ mvn compile
    $ mvn package

**Note** mvn clean target/dependency directory!

	
