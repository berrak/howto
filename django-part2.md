## Django part-2

In Komodo add the virtual environment location (i.e. ~/django) to a project, here named *django* in Komodo. The virtual environment root location contains:

	bin/
    include/
    lib/
    *wls/*
    django.project
    
Change the *project name* location initially given to *src*:

	bin/
    include/
    lib/
    *src/*
    
The source location (i.e. django project root) contains e.g.:

	wls/
    db.sqlite3
    manage.py
    LICENSE
    .gitignore
    README.md
    
The repository on GitHub:

https://github.com/codingforentrepreneurs/launch-with-code.git
    
## Configure Url

Open *url.py* in the *~/django/src/wls/* directory in Komodo. 

	from django.conf.urls import patterns, include, url
	from django.contrib import admin

	urlpatterns = patterns('',
    	# Examples:
    	url(r'^$', 'wls.views.home', name='home'),
    	# url(r'^blog/', include('blog.urls')),

    	url(r'^admin/', include(admin.site.urls)),
	)
    
Uncomment the *home* urlpattern view in *url.py*. To tell *python* that this folder is a package, create an empty *__init__.py* file unless it is already existing with:

	$ cd ~/django/src/wls/
	$ touch __init__.py
    
## Functional Views

Create *view.py*:

	from django.shortcuts import render

	def home(request):
    
    	context = {}    
    	template = 'home.html'
    	return render(request, template, context)

Function *renders* takes the *request* from *url*, and process the *dictionary context* and returns the rended page, based on the *template* file.

Temporarily leave this and let's have a look at *settings.py* in the part-3 of this series.






