## Python Developer Environment Setup

### Installing cookie cutter

Build from templates. Visit:

	https://github.com/audreyr/cookiecutter

Select a suitable template and clone it to a local git directory like so:

	mkdir CLONES
    cd ~/CLONES
    git clone https://github.com/audreyr/cookiecutter-pypackage.git

On Debian jessie, check status and install if missing required package:

	apt-cache policy python-pip
    apt-cache policy python-setuptools
    apt-cache policy python-wheel
    apt-cache policy python-yaml

Install missing packages:

	sudo aptitude install python-pip
    sudo aptitude install python-setuptools
    sudo aptitude install python-wheel
    sudo aptitude install python-yaml


Install cookiecutter from pip:

	sudo pip install markupsafe
    sudo pip install cookiecutter

### Create the new project

Before templating a new project edit the downloaded json-file:

	cd ~/CLONES
    cd /cookiecutter-pypackage
    vi cookiecutter.json

Now, create a new project directory *myproj*:

	cd ~/GIT
    mkdir myproj
    cd myproj
    cookiecutter ~/CLONES/cookiecutter-pypackage

Now, a complete GitHub ready project is generated in *myproj*.

### Installing python modules locally in a with virtual environment

Install *virtualenv* from pip. Virtual environments with this, are supported from 2.6 to 3.4+. This includes automatic install of pip and setuptools in the virtual environment.

	sudo pip install virtualenv
    
### Using the virtual environment

Check system python for jessie:

	which python
     /usr/bin/python
    python --version
     Python 3.4.2

Now we will create new local python3.4 environment:

	cd
    mkdir VIRTUAL_PY3.4
    
Initialising it:

	virtualenv VIRTUAL_PY3.4
	 New python executable in VIRTUAL_PY3.4/bin/python
	 Installing setuptools, pip, wheel...done.

Activate it i.e. allow us to install specific packages/libs only here:

	source VIRTUAL_PY3.4/bin/activate
     (VIRTUAL_PY3.4) ~$
     
Use pip to install *cookiecutter-pypackage* required developers (build) dependencies. Use the *requirements_dev.txt* file.

Some of required packages are python code extension which need som developer files:

	(VIRTUAL_PY3.4) sudo aptitude install python-dev
    (VIRTUAL_PY3.4) sudo aptitude install python3-dev
    (VIRTUAL_PY3.4) sudo aptitude install libffi-dev
    (VIRTUAL_PY3.4) sudo aptitude install libssl-dev

Now install with:

	(VIRTUAL_PY3.4) cd /home/bekr/GIT/myproj
	pip install -r requirements_dev.txt

Now try to build the package:

	(VIRTUAL_PY3.4) cd ~/GIT/myproj
	(VIRTUAL_PY3.4) make dist

This creates a source archieve and a wheel here:

	(VIRTUAL_PY3.4)bekr@dell:~/GIT/lipi$ ll dist
	 myproj-0.1.0-py2.py3-none-any.whl
	 myproj-0.1.0.tar.gz

Try a user install:

	(VIRTUAL_PY3.4) make install
	...
    ...
    Installing lipi script to /home/bekr/VIRTUAL_PY3.4/bin
    ...
    ...

Try to run the script:

	(VIRTUAL_PY3.4) which myproj
     /home/bekr/VIRTUAL_PY3.4/bin/myproj

    (VIRTUAL_PY3.4) myproj 123
		Python Hello from Myproj Team!
		Command line arguments is:
        ['/home/bekr/VIRTUAL_PY3.4/bin/myproj', '123']

That's it!

### De-activate the environment

To get back to normal:

	(VIRTUAL_PY3.4) deactivate

### Use the environment in an IDE

With PyCharm the virtual environment can be used as well. Download it from:

	https://www.jetbrains.com/pycharm/download/

Now create a new project in the new source tree and chose virtual environment directory from the drop down list of interpreters locations.

### Using the created make file

Before creating documents ensure that Sphinx and its depencies are installed.