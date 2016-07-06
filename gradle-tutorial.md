## Gradle for Java projects

Visit Gradle website for download:

https://gradle.org/downloads/

### Installation

Download latest version to users ~/tmp:

	https://services.gradle.org/distributions/gradle-2.4-bin.zip

Unzip the downloaded file to user $HOME directory:

	$ cd
    $ unzip -d $HOME tmp/gradle-2.4-bin.zip

### User configuration

Add to user *.bashrc* file:

```
# for gradle build system
export PATH=~/gradle-2.4/bin:$PATH
```	
Test the installation with:

	$ gradle -v

### Create *build.gradle* file

Create a working user directory for all maven based Java projects:

	$ mkdir ~/GRADLE
	$ cd GRADLE
	$ nano build.gradle

In the file add:

	// Buildfile for the fmw-tools project
	apply plugin: "java"

Show which tasks are available to us:

	$ gradle tasks

### Create required directory file structure

Now we need to create a standard directory tree like so:
The top level directory will be the project name, i.e. *fmw-tools*.
	
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

	$ mkdir -p fmw-tools/src/main/java/org/debinix/mwtools
    $ mkdir -p fmw-tools/test/main/java/org/debinix/mwtools

### Add Java sources

Add following code to *mwtools* in the src-subtree:

Add a file named *App.java*:

```
package org.debinix.mwtools;

public class App
{
    public static void main( String[] args )
    {
     	System.out.println( "Hello World!" );
    }
}
```
In the test-subtree add a file called *AppTest.java*:

```
package org.debinix.mwtools;

import junit.framework.Test;
import junit.framework.TestCase;
import junit.framework.TestSuite;

public class AppTest
    extends TestCase
{
    public AppTest( String testName )
    {
     	super( testName );
    }

    public static Test suite()
    {
     	return new TestSuite( AppTest.class );
    }

    public void testApp()
    {
        assertTrue( true );
    }
}
```

### Assemble project to package

	$ gradle assemble
```
:compileJava UP-TO-DATE
:processResources UP-TO-DATE
:classes UP-TO-DATE
:jar
:assemble

BUILD SUCCESSFUL

Total time: 3.283 secs
```

This creates an build output structure with the jar-archive.:

	$ tree

```
.
|-- build
|   |-- libs
|   |   `-- fmw-tools.jar
|   `-- tmp
|       `-- jar
|           `-- MANIFEST.MF
```
### Build package

	$ gradle build

The build will fail since build.gradle have not included any dependencies!

Update the *build.gradle* with this:

```
// Buildfile for the fmw-tools project
apply plugin: "java"
archivesBaseName = "fmw-tools"
version = '1.0-DEV'

repositories {
    mavenCentral()
}

dependencies {
    testCompile	group: 'junit',	name: 'junit', version:	'4.+'
}
```

	$ gradle build
    
```
:compileJava UP-TO-DATE
:processResources UP-TO-DATE
:classes UP-TO-DATE
:jar
:assemble
:compileTestJava
Download https://repo1.maven.org/maven2/junit/junit/4.12/junit-4.12.pom
Download https://repo1.maven.org/maven2/junit/junit/4.12/junit-4.12.jar
Download https://repo1.maven.org/maven2/org/hamcrest/hamcrest-core/1.3/hamcrest-core-1.3.jar
:processTestResources UP-TO-DATE
:testClasses
:test
:check
:build

BUILD SUCCESSFUL

Total time: 32.033 secs
```

### Directory structure

	$ tree
    
```
.
|-- build
|   |-- classes
|   |   `-- test
|   |       `-- org
|   |           `-- debinix
|   |               `-- mwtools
|   |                   `-- AppTest.class
|   |-- dependency-cache
|   |-- libs
|   |   |-- fmw-tools-1.0-DEV.jar
|   |   `-- fmw-tools.jar
|   |-- reports
|   |   `-- tests
|   |       |-- classes
|   |       |   `-- org.debinix.mwtools.AppTest.html
|   |       |-- css
|   |       |   |-- base-style.css
|   |       |   `-- style.css
|   |       |-- index.html
|   |       |-- js
|   |       |   `-- report.js
|   |       `-- packages
|   |           `-- org.debinix.mwtools.html
|   |-- test-results
|   |   |-- binary
|   |   |   `-- test
|   |   |       |-- output.bin
|   |   |       |-- output.bin.idx
|   |   |       `-- results.bin
|   |   `-- TEST-org.debinix.mwtools.AppTest.xml
|   `-- tmp
|       |-- compileTestJava
|       |   `-- emptySourcePathRef
|       `-- jar
|           `-- MANIFEST.MF
|-- build.gradle
`-- src
    |-- main
    |   `-- java
    |       `-- org
    |           `-- debinix
    |               `-- mwtools
    |                   `-- App.Java
    `-- test
        `-- java
            `-- org
                `-- debinix
                    `-- mwtools
                        `-- AppTest.java
```

### Run application

Run the application:

	$ cd build/libs
    $ java -jar fmw-tools-1.0-DEV
		No main manifest attribute, in fmw-tools-1.0-DEV

The problem is that we haven’t configured the main class of the jar file in the manifest file. Let’s find out how we can fix this problem.
        
### Configuring the Main Class of a Jar File

The Java plugin adds a jar task to our project, and every jar object has a manifest property which is an instance of Manifest.

We can configure the main class of the created jar file by using the attributes() method of the Manifest interface. In other words, we can specify the attributes added to the manifest file by using a map which contains key-value pairs.

We can set the entry point of our application by setting the value of the Main-Class attribute.

Add following to *build.gradle*:

```
apply plugin: 'application'
mainClassName = 'org.debinix.mwtools.App'

jar {
    manifest {
        attributes 'Main-Class': 'org.debinix.mwtools.App'
    }
}
```

Now re-build application.

	$ gradle clean
    $ gradle build

THIS IS SUPPOSE TO WORK - BUT IT DOESNT. WERE IS THE CLASS DIRECTORY!

