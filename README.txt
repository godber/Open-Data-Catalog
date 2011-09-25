Recommended stack:
    Apache2
    mod-wsgi
    Python 2.6+
    PostgreSQL 8.4
    Django 1.3
    
Dependancies from aptitide:
    git libapache2-mod-wsgi sendmail postgresql-8.4

Python dependencies:
    Dependencies are best installed in a virtualenv, so as to not conflict
    with other local development.
    Install dependencies via pip:
        pip install -r requirements.txt
    There's a problem with the dbfpy package at the time of this writing;
    download at:
        http://sourceforge.net/projects/dbfpy/files/
    then install:
        pip install dbfpy-2.2.5.tar.gz

	
Update apache2 conf
	/etc/apache2/sites-available/default add >
		WSGIScriptAlias /hidden /<project location>/odp.wsgi
        Alias /media /<project location>/media
        Alias /static /<project location>/static

	create /<project location>/odp.wsgi >
		import os, sys
		sys.path.insert(0, '/home/azavea/NPower_OpenDataPhilly')

		import settings

		import django.core.management
		django.core.management.setup_environ(settings)
		utility = django.core.management.ManagementUtility()
		command = utility.fetch_command('runserver')

		command.validate()

		import django.conf
		import django.utils

		django.utils.translation.activate(jangod.conf.settings.LANGUAGE_CODE)

		import django.core.handlers.wsgi

		application = django.core.handlers.wsgi.WSGIHandler()
		
		
Setup source
	Clone project
	
	Make media directory:
		mkdir media
		chmod 775 media
	Make static directory:
		mkdir static
		chmod 775 static
	Create a symbolic link to admin media:
		ln -s /usr/local/lib/python2.7/dist-packages/django/contrib/admin admin_media
	Sync database:
		pythong manage.py syncdb
	Collect static files:
		python manage.py collectstatic

