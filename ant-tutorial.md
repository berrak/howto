## Ant for Java projects

Visit ant website:

http://ant.apache.org/

### Installation

Download latest version and follow the installation procedure given (i.e. untar archive in */usr/local* as root).

	# cd /usr/local
	# tar -xzvf apache-ant-1.9.5-bin.tar.gz

### User configuration

Add to user *.bashrc* file:

```
# for ant build system
export ANT_HOME=/usr/local/apache-ant-1.9.5
export PATH=${PATH}:${ANT_HOME}/bin

```	
Test the installation as regular user.
	$ ant
    Buildfile: build.xml does not exist!
	Build failed

Which is normal since the build-file is missing.

### Set up project *fmt-tools*

Create a working user directory for all ant based Java projects (**Note**: Do not use hyphens in package/directory names below the project name!):

	$ mkdir -p ~/ANT/fmw-tools
	$ cd ANT/fmw-tools
	$ mkdir -p src/main/java/org/debinix/fmwcore
    $ mkdir -p build/classes

In the following description ${PROJECT_HOME} is the same as *~/ANT/fmw-tools*.

### Add Java source

	$ cd src/main/java/org/debinix/fmw-core
    
In aditor add file *Core.java*:

```
package org.debinix.fmwcore;
public class Core {
    public static void main(String[] args) {
        System.out.println("Hello World");
    }
}
```
The top level directory is the project name, i.e. *fmw-tools*. The *classes* subtree is created by the complier when we compile.

	$ cd ~/ANT/fmw-tools
    $ tree

```
|-- build
|   `-- classes
`-- src
    `-- main
        `-- java
            `-- org
                `-- debinix
                    `-- fmwcore
                        `-- Core.java
```

### Compile and run manually

	$ cd ~/ANT/fmw-tools
	$ javac -sourcepath src/main/java -d build/classes src/main/java/org/debinix/fmwcore/Core.java

	$ tree

Now the build/classes subtree is present.

```
.
|-- build
|   `-- classes
|       `-- org
|           `-- debinix
|               `-- fmwcore
|                   `-- Core.class
```

Run with set classpath:

	$ java -cp build/classes org.debinix.fmwcore.Core
	Hello World!

or change to the class directory:

	$ cd build/classes
	$ java org.debinix.fmwcore.Core
    Hello world!

### Create a startable jar-file

Creating a jar-file is not very difficult. But creating a startable jar-file needs more steps: create a manifest-file containing the start class, creating the target directory and archiving the files.

	$ echo "Main-Class: org.debinix.fmwcore.Core" > myManifest
	$ mkdir build/jar
	$ jar cfm build/jar/fmw-tools.jar myManifest -C build/classes .

	$ tree

```
.
|-- build
|   |-- classes
|   |   `-- org
|   |       `-- debinix
|   |           `-- fmwcore
|   |               `-- Core.class
|   `-- jar
|       `-- fmw-tools.jar
|-- myManifest
```


Run it:

	$ java -jar build/jar/fmw-tools.jar
	Hello world!

### Build the application with Ant

Create a *build.xml* for Ant:

```
<project>

    <target name="clean">
        <delete dir="build"/>
    </target>

    <target name="compile">
        <mkdir dir="build/classes"/>
        <javac srcdir="src/main/java/org/debinix/fmwcore" destdir="build/classes"/>
    </target>

    <target name="jar">
        <mkdir dir="build/jar"/>
        <jar destfile="build/jar/fmw-tools.jar" basedir="build/classes">
            <manifest>
                <attribute name="Main-Class" value="org.debinix.fmwcore.Core"/>
            </manifest>
        </jar>
    </target>

    <target name="run">
        <java jar="build/jar/fmw-tools.jar" fork="true"/>
    </target>

</project>
```
Now you can compile, package and run the application via

	$ ant compile
	$ ant jar
	$ ant run

Or shorter with

	$ ant compile jar run

All works as expected the *ant run* produce:

```
Buildfile: /home/bekr/ANT/fmw-tools/build.xml
run:
     [java] Hello World
BUILD SUCCESSFUL
Total time: 0 seconds
```

### Improve the *build.xml* file

Now we have a working buildfile we could do some enhancements: many time you are referencing the same directories, main-class and jar-name are hard coded, and while invocation you have to remember the right order of build steps.

The first and second point would be addressed with properties, the third with a special property - an attribute of the <project>-tag and the fourth problem can be solved using dependencies.

Change the *build.xml* to:

```
<project name="fmw-tools" basedir="." default="main">

    <property name="src.dir"     value="src/main/java/org/debinix/fmwcore"/>

    <property name="build.dir"   value="build"/>
    <property name="classes.dir" value="${build.dir}/classes"/>
    <property name="jar.dir"     value="${build.dir}/jar"/>

    <property name="main-class"  value="org.debinix.fmwcore.Core"/>


    <target name="clean">
        <delete dir="${build.dir}"/>
    </target>

    <target name="compile">
        <mkdir dir="${classes.dir}"/>
        <javac srcdir="${src.dir}" destdir="${classes.dir}"/>
    </target>

    <target name="jar" depends="compile">
        <mkdir dir="${jar.dir}"/>
        <jar destfile="${jar.dir}/${ant.project.name}.jar" basedir="${classes.dir}">
            <manifest>
                <attribute name="Main-Class" value="${main-class}"/>
            </manifest>
        </jar>
    </target>

    <target name="run" depends="jar">
        <java jar="${jar.dir}/${ant.project.name}.jar" fork="true"/>
    </target>

    <target name="clean-build" depends="clean,jar"/>

    <target name="main" depends="clean,run"/>

</project>
```

No run with:

	$ ant

```
Buildfile: /home/bekr/ANT/fmw-tools/build.xml

clean:
   [delete] Deleting directory /home/bekr/ANT/fmw-tools/build

compile:
    [mkdir] Created dir: /home/bekr/ANT/fmw-tools/build/classes
    [javac] /home/bekr/ANT/fmw-tools/build.xml:18: warning: 'includeantruntime' was not set, defaulting to build.sysclasspath=last; set to false for repeatable builds
    [javac] Compiling 1 source file to /home/bekr/ANT/fmw-tools/build/classes

jar:
    [mkdir] Created dir: /home/bekr/ANT/fmw-tools/build/jar
      [jar] Building jar: /home/bekr/ANT/fmw-tools/build/jar/fmw-tools.jar

run:
     [java] Hello World

main:

BUILD SUCCESSFUL
Total time: 1 second
```

### External dependencies

Modify our source file:

```
package org.debinix.fmwcore;

import org.apache.log4j.Logger;
import org.apache.log4j.BasicConfigurator;

public class Core {
    static Logger logger = Logger.getLogger(Core.class);

    public static void main(String[] args) {
        BasicConfigurator.configure();
        logger.info("Hello World!");
    }
}
```
Log4J is not inside the classpath so we have to do a little work here. But do not change the CLASSPATH environment variable! This is only for this project and maybe you would break other environments (this is one of the most famous mistakes when working with Ant). We introduce Log4J (or to be more precise: all libraries (jar-files) which are somewhere under *lib*) into our buildfile:

Create the *lib* directory:

	$ mkdir ~/ANT/fmw-tools/lib

Download *log4j* (or the new *log4j 2*) and un-tar it:

	$ cd /tmp
    $ tar -xzvf log4j-1.2.17.tar.gz
    $ mv apache-log4j-1.2.17/*.jar ~/ANT/fmw-tools/lib/

Add *lib.dir* property and define a *classpath* id. The reference that in task *compile* and *run*.
The updated *build.xml* looks like:

```
<project name="fmw-tools" basedir="." default="main">

    <property name="src.dir"     value="src/main/java/org/debinix/fmwcore"/>

    <property name="build.dir"   value="build"/>
    <property name="classes.dir" value="${build.dir}/classes"/>
    <property name="jar.dir"     value="${build.dir}/jar"/>

    <property name="main-class"  value="org.debinix.fmwcore.Core"/>

    <property name="lib.dir"     value="lib"/>

    <path id="classpath">
        <fileset dir="${lib.dir}" includes="**/*.jar"/>
    </path>

    <target name="clean">
        <delete dir="${build.dir}"/>
    </target>

    <target name="compile">
        <mkdir dir="${classes.dir}"/>
        <javac srcdir="${src.dir}" destdir="${classes.dir}" classpathref="classpath"/>
    </target>

    <target name="jar" depends="compile">
        <mkdir dir="${jar.dir}"/>
        <jar destfile="${jar.dir}/${ant.project.name}.jar" basedir="${classes.dir}">
            <manifest>
                <attribute name="Main-Class" value="${main-class}"/>
            </manifest>
        </jar>
    </target>

    <target name="run" depends="jar">
        <java fork="true" classname="${main-class}">
            <classpath>
                <path refid="classpath"/>
                <path location="${jar.dir}/${ant.project.name}.jar"/>
            </classpath>
        </java>
    </target>

    <target name="clean-build" depends="clean,jar"/>

    <target name="main" depends="clean,run"/>

</project>
```
In this example we start our application not via its Main-Class manifest-attribute, because we could not provide a jarname **and** a classpath. So add our class in the red line to the already defined path and start as usual.

	$ ant

```
Buildfile: /home/bekr/ANT/fmw-tools/build.xml

clean:
   [delete] Deleting directory /home/bekr/ANT/fmw-tools/build

compile:
    [mkdir] Created dir: /home/bekr/ANT/fmw-tools/build/classes
    [javac] /home/bekr/ANT/fmw-tools/build.xml:23: warning: 'includeantruntime' was not set, defaulting to build.sysclasspath=last; set to false for repeatable builds
    [javac] Compiling 1 source file to /home/bekr/ANT/fmw-tools/build/classes

jar:
    [mkdir] Created dir: /home/bekr/ANT/fmw-tools/build/jar
      [jar] Building jar: /home/bekr/ANT/fmw-tools/build/jar/fmw-tools.jar

run:
     [java] 0 [main] INFO org.debinix.fmwcore.Core  - Hello World!

main:

BUILD SUCCESSFUL
Total time: 1 second
```

### Use *log4j* configuration file

Log4j can be configured with a default *log4j.properties* file. Modify the *Core.java* like so:

```
package org.debinix.fmwcore;
import org.apache.log4j.Logger;

public class Core {
    static Logger logger = Logger.getLogger(Core.class);

    public static void main(String[] args) {
        logger.info("Hello World!");
    }
}
```

In the same directory as the *Core.java* create a file *log4j.properties*:

```
log4j.rootLogger=DEBUG, stdout
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%m%n
```

Add to *build.xml* compile target which will copy the *log4j.properties* file to the build subtree:

```
<copy todir="${classes.dir}">
   <fileset dir="${src.dir}" excludes="**/*.java"/>
</copy>
```

Run again from *~/ANT/fmw-tools* with:

	$ ant
    Hello world!

or with (on Unix/Linux, Windows use ; in the classpath!):

	$  java -cp "build/classes:lib/*" org.debinix.fmwcore.Core
	Hello world!


### Testing framworks

Start by creating a new test subtree from ${PROJECT_HOME}:

	$ mkdir -p test/java/org/debinix/fmwcore

In that test directory create a file *CoreTest.java*

	$ cd test/java/org/debinix/fmwcore

```
public class CoreTest extends junit.framework.TestCase {

    public void testNothing() {
    }

    public void testWillAlwaysFail() {
        fail("An error message");
    }
}

```

Download *junit.jar* and *hamcrest-core.jar* and place these in ${PROJECT_HOME}/lib directory.








