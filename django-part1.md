## DJANGO

	# aptitude install virtualenv python-pip

Create a virtual environment for the python project.

	$ mkdir django
    $ cd django
    $ source bin/activate

The last command activates the virtualenvironment. Download same Django version as *jessie* supports i.e. 1.7.7 with:

	$ pip install django==1.7.7
    $ pip freeze

Last commad shows all installed (from pip) versions.

	$ django-admin.py startproject wls
    $ cd wls
    $ ls
    	wls      manage.py
        
Start the server with:

	$ python manage.py runserver
    	validating models....
        Starting development server 127.0.0.1:8000
        Quit the server with Ctrl-C.

Use the Url in a browser to confirm a working server.

### Create database (SQLite) for users

	$ python manage.py start syncdb
    	Creating tables...
        ...
        Create super user now (y/N)?

On the last question just add yourself as admin.

Now try the new admin Url:

	127.0.0.1:8000/admin/
    
Try to login to the new Django site.

