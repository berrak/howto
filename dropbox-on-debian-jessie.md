## Dropbox on Debian 8 (Jessie)

Dropbox was founded in 2007 by Drew Houston and Arash Ferdowsi, two MIT students tired of emailing files to themselves to work from more than one computer.

Today, more than 200 million people across every continent use Dropbox to always have their stuff at hand, share with family and friends, and work on team projects.

After the DropBox Package Installation for Debian Linux Follow the Quick-Start Guide to Complete the DropBox SetUp.
Getting-Started with DropBox Debian Jessie 8 - Featured

### Download Latest DropBox Package for Debian Linux

Browse to Dropbox website and download Debian package:

	https://www.dropbox.com/install?os=lnx

Install DropBox for Debian:

    $ cd $HOME/Downloads/
    $ su root
      ********
    # aptitude install gdebi
    # gdebi ./dropbox*.deb
	Reading package lists... Done
	Building dependency tree        
	Reading state information... Done
	Building data structures... Done 
	Building data structures... Done 

	cloud synchronization engine - CLI and Nautilus extension
	 Dropbox is a free service that lets you bring your photos, 	docs, and videos
 	anywhere and share them easily.
 	.
 	This package provides a command-line tool and a Nautilus 		extension that
 	integrates the Dropbox web service with your GNOME Desktop.
	Do you want to install the software package? [y/N]:y
	Selecting previously unselected package dropbox.
	(Reading database ... 187619 files and directories currently 	installed.)
	Preparing to unpack dropbox_2015.02.12_amd64.deb ...
	Unpacking dropbox (2015.02.12) ...
	Setting up dropbox (2015.02.12) ...
	Please restart all running instances of Nautilus, or you 		will experience problems. i.e. nautilus --quit
	Dropbox installation successfully completed! You can start 		Dropbox from your applications menu.
	Processing triggers for gnome-menus (3.13.3-6) ...
	Processing triggers for desktop-file-utils (0.22-1) ...
	Processing triggers for mime-support (3.58) ...
	Processing triggers for hicolor-icon-theme (0.13-1) ...
	Processing triggers for man-db (2.7.0.2-5) ...

Install Additional GPG Verify Package (Optional):

    # aptitude install python-gpgme

Now a Dropbox folder is available in *$HOME/Dropbox/* which
is syncronized to Dropbox site.

Download this Python script (https://www.dropbox.com/download?dl=packages/dropbox.py) to control Dropbox from the command line. For easy access, put a symlink to the script anywhere in your PATH.

Options available:

     status       get current status of the dropboxd
     help         provide help
     puburl       get public url of a file in your dropbox
     stop         stop dropboxd
     running      return whether dropbox is running
     start        start dropboxd
     filestatus   get current sync status of one or more files
     ls           list directory contents with current sync 					  status
     autostart    automatically start dropbox at login
     exclude      ignores/excludes a directory from syncing
     lansync      enables or disables LAN sync



### Configure Dropbox

	$ chmod u+x dropbox.py 
	$ ./dropbox.py start -i
    
Enter your email and password when asked for. The Dropbox folder is now created and all files are synced (i.e. downloaded to the local folder).

### Selective sync

Select which folders to sync on Linux

    Click on the Dropbox icon from the menu bar.
    Select Preferences.
    Click the Account tab.
    Click the Selective Sync button.

A window will appear with a list of all the top level folders in your Dropbox folder. The folders with a check next to them will be synced to your computer. Uncheck any folders that you don't need to sync to your computer's hard drive. When you're done, select OK. Any folders you deselected will be removed from your hard drive, but will still be available through the website and on any computers linked to your Dropbox account.

Use the Advanced View button to drill down into the folders in your Dropbox. Click on the arrow next to the folders in your Dropbox to drill down and check or uncheck folders deep within your Dropbox hierarchy.
### From the command line

If you have the Dropbox command line instructions script, then all you need to do to add a folder to the Selective Sync "do not sync" list is enter the following command in your Terminal.

	/path/to/dropbox.py exclude add ~/Dropbox/path/to/folder/

Simply substitute the /path/to/ to the actual paths to the CLI script and the folder or file, respectively.

### Control syncing process

Dropbox allows you to pause and resume syncing through the Dropbox menu in your system tray or menu bar.

If you'd like to stop Dropbox entirely, you can do so through an option in your Dropbox menu.

### Deleting files (locally and remote)

When you delete a file from your Dropbox it is removed from your visible folder structure (the folders you see in your Dropbox). However, the file is not yet permanently deleted. Rather, it's moved into a queue for permanent deletion (see below for info specific to your account type). This queuing enables features like file recovery and version history.

For most Basic and Pro users, files are recoverable for 30 days