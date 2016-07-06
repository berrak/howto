## Java directory layout in JDeveloper

Assumption is that code will be on GitHub.

Create the main JDeveloper root directory eg:

	$ mkdir ~/JAVA_GIT

The repo on GitHub will be called *fmw-tools*:

	$ mkdir ~/JAVA_GIT/fmw-tools

This name is also used as the JDeveloper's application name. In that usage it's the root level of the hiearchy. Below this directory multiple projects may exist. Prename the project *Interpreter*

Following the accepted file structure conventions, create:

	$ mkdir -p ~/JAVA_GIT/fmw-tools/Interpreter/src/main/java
    $ mkdir -p ~/JAVA_GIT/fmw-tools/Interpreter/src/test/java

Add the organisation (i.e. your organisation url backwards)

	$ cd ~/JAVA_GIT/fmw-tools/Interpreter/src/main/java
    $ mkdir -p org/debinix
    $ cd ~/JAVA_GIT/fmw-tools/Iterpreter/src/test/java
    $ mkdir -p org/debinix

Finally, start JDeveloper (here installed in user home directory).

	$ bash ~/oracle_home/jdeveloper/jdev/bin/jdev

In the menu *appliocation->New* select:

	* Custom Application
    * OK

In the fields enter/edit suggestions:

	* Application Name: fmw-tools
    * Directory: /home/bekr/JAVA/fwm-tools/Interpreter/src/main/java
    * Application Package Prefix: org.debinix

In the next screen:

	* Project name: Interpreter
    * Directory: *as above*
    
Add features:

	* Java
    * Ant
    









