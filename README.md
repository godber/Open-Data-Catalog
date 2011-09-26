Recommended stack
-----------------

- Apache2
- mod-wsgi
- Python 2.6+
- PostgreSQL 8.4
- Django 1.3
    
Dependancies from aptitide
--------------------------

- git
- libapache2-mod-wsgi
-  postgresql-8.4
- python-psycopg2
- python-imaging 
- python-simplejson
- python-httplib2
- python-simpletools
- python-twitter

Dependancies from web
---------------------

- Django 1.3:           wget http://www.djangoproject.com/download/1.3/tarball/
- dbfpy:                wget http://sourceforge.net/projects/dbfpy/files/dbfpy/2.2.5/dbfpy-2.2.5.tar.gz
- sorl:                 wget http://thumbnail.sorl.net/sorl-thumbnail-3.2.5.tar.gz
- django-registration:  wget https://bitbucket.org/ubernostrum/django-registration/downloads/django-registration-0.8-alpha-1.tar.gz
- django-pagination:    wget http://django-pagination.googlecode.com/files/django-pagination-1.0.5.tar.gz
- django-ratings:       wget https://github.com/dcramer/django-ratings/tarball/d0ae149d112c7e5f9f40bb04028ceac4bf4b0e0d
- python-oauth2:        wget https://github.com/simplegeo/python-oauth2/tarball/master
- pytz:                 pip install pytz

Dependancies that require patching
----------------------------------

- git clone git://github.com/directeur/django-sorting.git
    - Apply patch to django-sorting: https://github.com/directeur/django-sorting/issues#issue/8
        - including comment by Alsaihn
    - sudo cp django-sorting -R /usr/local/lib/python2.6/dist-packages/django_sorting

Update apache2 conf
-------------------

- /etc/apache2/sites-available/default add >  

        WSGIScriptAlias /hidden /<project location>/odp.wsgi
        Alias /media /<project location>/media
        Alias /static /<project location>/static

- create /&lt;project location&gt;/odp.wsgi >  

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
------------

- Clone project
- Make media directory:
    - mkdir media
    - chmod 775 media
- Make static directory:
    - mkdir static
    - chmod 775 static
- Create a symbolic link to admin media:
    - ln -s /usr/local/lib/python2.7/dist-packages/django/contrib/admin admin_media
- Sync database:
    - python manage.py syncdb
- Collect static files:
    - python manage.py collectstatic
