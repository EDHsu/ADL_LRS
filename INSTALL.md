If you would like to manually install the LRS, rather than use the provided script, follow the instructions below.

## Installation

**Install Prerequisites**

    admin:~$ sudo apt-get install git fabric postgresql python-setuptools \
    	postgresql-server-dev-all python-dev libxml2-dev libxslt-dev
    admin:~$ sudo easy_install pip
    admin:~$ sudo pip install virtualenv
    
**Create ADL LRS system user**

    admin:~$ sudo useradd -c "ADL Learning Record Store System" -m -s "/bin/bash" <db_owner>
    
**Setup Postgres**

    admin:~$ sudo -u postgres createuser -P <db_owner>
    Enter password for new role: *****
	Enter it again: *****
    Shall the new role be a superuser? (y/n) y
    admin:~$ sudo -u postgres psql template1
    template1=# CREATE DATABASE lrs OWNER <db_owner>;
    template1=# \q (exits shell)
    
**Clone the LRS repository**

	admin:~$ sudo su <db_owner>
	dbowner:~$ mkdir <dir_name>
    dbowner:~$ git clone https://github.com/adlnet/ADL_LRS.git <dir_name>/
    dbowner:~$ cd <dir_name>/ADL_LRS
    
**Set the LRS configuration**

	### File: ADL_LRS/adl_lrs/settings.py
	
	# configure the database
	DATABASES = {
    	'default': {
    	    'ENGINE': 'django.db.backends.postgresql_psycopg2',
	        'NAME': 'lrs',
	        'USER': '<db_owner>',
	        'PASSWORD': '<password>',   # Comment out these lines if
	        'HOST': 'localhost',      # using postgresql "peer" auth.
	        'PORT': '',               # See pg_hba.conf for details
	    }
	}
	
	# Make this unique, and don't share it with anybody.
	SECRET_KEY = 'Some long random string with numb3rs and $ymbol$'
	
	# set to 'https' if using SSL encryption
	SITE_SCHEME = 'http'

**Setup the environment**

    dbowner:ADL_LRS$ fab setup_env
    ...
    dbowner:ADL_LRS$ source ../env/bin/activate
    (env)dbowner:ADL_LRS$
    
**Setup the LRS**

    (env)dbowner:ADL_LRS$ fab setup_lrs
    ...
    You just installed Django's auth system, which means you don't have any superusers defined.
	Would you like to create one now? (yes/no): yes
	Username (leave blank to use '<db_owner>'): 
	E-mail address:
	Password: 
	Password (again): 
	Superuser created successfully.
	...

If you get some sort of authentication error here, make sure that Django and PostgreSQL are both
using the same form of authentication (*adl_lrs/settings.py* and *pg_hba.conf*) and that the credentials
given in *settings.py* are the same as those you created.

## Starting

While still in the ADL_LRS directory, run

    (env)dbowner:ADL_LRS$ python manage.py runserver

This starts a lightweight development web server on the local machine. By default, the server runs on port 8000 on the IP address 127.0.0.1. You can pass in an IP address and port number explicitly. This will serve your static files without setting up Nginx but must NOT be used for production purposes. Press `CTRL + C` to stop the server


Set your site domain

  Visit the admin section of your website (/admin). Click Sites and you'll see the only entry is 'example.com' (The key for this in the DB is 1 and it maps back to the SITE_ID value in settings). Change the domain and name to the domain you're going to use. If running locally it could be localhost:8000, or if production could be lrs.adlnet.gov (DON'T include the scheme here, that should be set in settings.py already). Sync your database again to apply the change

    (env)dbowner:ADL_LRS$ python manage.py syncdb



Whenever you want to exit the virtual environment, just type `deactivate`


For other ways to start and run the LRS, please visit our Wiki.

## Test LRS
    
    (env)dbowner:ADL_LRS$ fab test_lrs

## Helpful Information
    
* [Test Coverage](https://github.com/adlnet/ADL_LRS/wiki/Code-Coverage)
* [Code Profiling](https://github.com/adlnet/ADL_LRS/wiki/Code-Profiling-with-cProfile)
* [Sending Attachments](https://github.com/adlnet/ADL_LRS/wiki/Sending-Statements-with-Attachments)
* [Setting up Nginx and uWSGI](https://github.com/adlnet/ADL_LRS/wiki/Using-Nginx-for-Production)
* [OAuth Help](https://github.com/adlnet/ADL_LRS/wiki/Using-OAuth)
* [Clearing the Database](https://github.com/adlnet/ADL_LRS/wiki/Clearing-the-Database)
