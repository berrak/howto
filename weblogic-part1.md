## Weblogic install on OL7

Following (Middleware Home) directory location is used in this howto:

* $MIDDLEWARE_HOME=/home/oracle/middleware

On the server create a dedicated user *oracle*:

	# groupadd oracle
    # useradd -g oracle oracle
    # passwd oracle
    
Log in as user *oracle* and change permissions:

	umask 027

Now download *Weblogicserver* from Oracle :

* fmw_12.1.3.0.0_wls.jar

Check that java is installed:

    $ java -version

Otherwise, download java and install with:

	# rpm -Uvh jdk-8u45-linux-x64.rpm
    
Set $MIDDLEWARE_HOME (named *Oracle home* in the installer) as: /home/oracle/middleware/12.1.3.0.0-0  (the last digit is lokal patch). Run the install wizzard:

	$ java -jar fmw_12.1.3.0.0_wls.jar

Ignore the warning about OracleLinux 7 is not supported.
Save the responsefile:

	~/weblogicserver_12.1.3-0.rc

With the response file you can run the installer like so:

	$ java -jar fmw_12.1.3.0.0_wls.jar -silent
           -responseFile weblogicserver_12.1.3-0.rc

Run the inventory setup file in ~/oraInventory as root:

	$ su -c './createCentralInventory.sh'
    
Note that with the uninstall.sh, the oraInventory-file is not removed in /etc. thus have tio be removed manually.

## Domains

Do not set up the domains in a subdirectory to $MIDDLEWARE_HOME.

Create a domain directory structure below ~/domains

	$ mkdir ~/domains



Domains: /home/fmw/domains/P_xxx (P: Production, D: Development)
