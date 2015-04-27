﻿# ADL LRS 

#### Installation tested on Ubuntu 12.10 machine with Python 2.7.3. Should be good with Ubuntu 10.04 LTS - 13.04 releases. Updated to be compliant with the 1.0.2 xAPI spec.

This version is stable, but only intended to support a small amount of users as a proof of concept. While it uses programming best practices, it is not designed to take the place of an enterprise system.

## Quick Installation

(Optional) Prior to cloning the repository, create an ADL LRS system user and switch to that user:

```
sudo su

useradd -c "ADL Learning Record Store System" -m -s "/bin/bash" adllrs

<<<<<<< HEAD
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
=======
su adllrs

cd ~
```

change to root and run setup_lrs.sh:

```
sudo su
./setup_lrs.sh
```

For development, you can run the LRS with:

```
python manage.py runserver
```

Alternatively, follow the instructions below.
>>>>>>> added setup_lrs.sh for quick install

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

## License
   Copyright &copy;2015 Advanced Distributed Learning

   Licensed under the Apache License, Version 2.0 (the "License");
   you may not use this file except in compliance with the License.
   You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and
   limitations under the License.
