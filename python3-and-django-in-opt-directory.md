### Django using Python 3.3 on Debian wheezy - how to

Debian 7 comes with python 2.7.3, and python 3.2.3

To use python2.7.3, we use the command "python" and to use python3.2.3 we use "python3"

The goal is we want to use python3.3.x. Why? you asked. That's a very good question!
The reason is because I want to use MySQL as the database and MySQL Connector Python 2.0.1 only supports Debian 6.

I have to use the platform independent version of MySQL Connector Python instead of the Debian version.

So, first I have to install Python3.3 from source and this page (http://askubuntu.com/questions/244544/how-do-i-install-python-3-3)  shows me how.

I am going to tell you how anyway.

	sudo apt-get install build-essential
	sudo apt-get install libsqlite3-dev
	sudo apt-get install sqlite3
	sudo apt-get install bzip2 libbz2-dev

Cool! then I download and compile Python3.3

	
	cd /home/me/downloads
	wget http://www.python.org/ftp/python/3.3.5/Python-3.3.5.tar.xz
	tar xJf ./Python-3.3.5.tar.xz
	cd Python-3.3.5
	./configure --prefix=/opt/python3.3
	make && sudo make install

I prefer to use alias for Python3.3 and I called it "py"

echo 'alias py="/opt/python3.3/bin/python3.3"' >> .bashrc

That's it! I now have Python3.3 and the next step is to install Django 1.7. But before we do that, we must first install "distribute" and here how I did it. "Distribute" can be found on this page (http://kalianto.blogspot.se/2014/10/django-using-python33-on-debian-wheezy.html)
	
	cd /home/me/downloads
	wget 		https://pypi.python.org/packages/source/d/distribute/distribute-0.7.3.zip#md5=c6c59594a7b180af57af8a0cc0cf5b4a
	unzip distribute-0.7.3.zip
	cd distribute-0.7.3
	py setup.py install

Now we have distribute package which is what we need to install Django 1.7. Without this package, it will throw an ImportError: "No module name setuptools"
Get the latest release of Django 1.7 here https://www.djangoproject.com/download/

	cd /home/me/downloads
	wget https://www.djangoproject.com/download/1.7/tarball/
	tar -zxvf Django-1.7.tar.gz
	cd Django-1.7
	py setup.py install

Great! I now have Django 1.7 installed. NOTE: when using django-admin.py, you must call it using "py" instead of just "django-admin.py" as mentioned in the Django tutorial (https://docs.djangoproject.com/en/1.7/intro/tutorial01/).
Find out where "django-admin.py" was installed here:

	locate django-admin.py
	/usr/local/bin/django-admin.py

Last step is to install MySQL Connector Python, well, because I want to use MySQL. That's the reason why this post is created.
Download MySQL Connector Python here (http://dev.mysql.com/downloads/connector/python/)
	
	cd /home/me/downloads
	wget http://dev.mysql.com/get/Downloads/Connector-Python/mysql-	connector-python-2.0.1.tar.gz
	tar -zxvf mysql-connector-python-2.0.1.tar.gz
	cd mysql-connector-python-2.0.1
	py setup.py install

Now you can start a new project by calling
	
	py /usr/local/bin/django-admin.py startproject mysite

To use MySQL Connector Python, edit your settings.py as below
	
	DATABASES = {
    	'default': {
        	'ENGINE'  : 'mysql.connector.django',
	        'NAME'    : 'mysite',
       	 'USER'    : 'root',
     	   'PASSWORD': 'IShallNotUseRoot',
     	   'HOST'    : '127.0.0.1',
    	    'PORT'    : '3306'
  	  }
	}

REMEMBER: Use "py" (in my case) for all the call, such as py manage.py runserver.
Hope this helps you!