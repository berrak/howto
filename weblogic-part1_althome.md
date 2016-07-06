## Oracle Fusion Middleware Install on OL7

Following (Middleware Home) directory location is used in this howto:

* $ORACLE_HOME=/opt/oracle/middleware/12.1.3

On the server create a dedicated user *oracle*:

	# groupadd oracle
    # useradd -g oracle oracle
    # passwd oracle

Create the directory structure and update permissions:

	# mkdir -p /opt/oracle
    # chown -R oracle:oracle /opt/oracle
    # chmod ug+rw /opt/oracle 

Use the graphical login and as user *oracle*, otherwise installer complains about missing DISPLAY. Download *Fusion Middleware* from Oracle :

* fmw_12.1.3.0.0_wls.jar

Check that java is installed:

    $ java -version

Otherwise, download java and install with:

	# rpm -Uvh jdk-8u45-linux-x64.rpm

### Install FMW 12.1.3

Run the install wizzard (but skip the final *launch quick setup which creates domains and servers*). Set Inventory directory till */opt/oracle* and use $ORACLE_HOME (aka MW_HOME) as:
/home/oracle/middleware/12.1.3.0.0

	$ java -jar fmw_12.1.3.0.0_wls.jar

Ignore the faulty warning about *OracleLinux 7 is not supported*.
Save the responsefile:

	~/weblogicserver_12.1.3-0.rc

With the response file you can run the installer like so:

	$ java -jar fmw_12.1.3.0.0_wls.jar -silent
           -responseFile weblogicserver_12.1.3-0.rc

Run the inventory setup file in ~/oraInventory as root:

	$ su -c './createCentralInventory.sh'
    
Note that with the uninstall.sh, the oraInventory-file is not removed in /etc. thus have tio be removed manually.

### Domains and WLST

Do not set up the domains in the subdirectory of $ORACLE_HOME.

Maintain the domain directory structure below:

	/home/oracle
    
The WLST script use the *domain template wls.jar* to create the first domain and the admin server.

Domains: /home/oracle/domains/yy_Pnnn (P: Poduction, D: Development)
