### Building Debian packages

Install prerequisites

	# apt-get install packaging-tutorial dh-make debhelper devscripts

Then, open and skim the PDF located at /usr/share/doc/packaging-tutorial/packaging-tutorial.pdf. After skimming it you'll have the basic knowledge needed to understand the structure of a Debian package.

Create working directory

	$ mkdir ~/debianwork
    $ cp bdeb-0.1.tar.gz ~/debianwor/.
    $ cd  ~/debianwork
    
Unpack the archive:

	$ tar -xzvf bdeb-0.1.tar.gz
	$ cd bdeb-0.1
    
Create the debian directory with
    
	$ dh_make --createorig
    
