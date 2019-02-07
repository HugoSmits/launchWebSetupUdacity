# AWS lightSail

## Description

#### The task is to launch the catalogUdacity project to the internet using Amazon Web Service with lightsail. A linux instance will be created and configured using the steps listed below.

## Logging into AWS and using Lightsail
1. #### Log into your Amazon account.
2. #### Create an instance
	A Lightsail instance is a Linux server running on a virtual machine inside an Amazon datacenter.
3. #### Choose an instance image: Ubuntu
	Lightsail supports a lot of different instance types. An instance image is a particular software setup, including an operating system and optionally built-in applications.
	For this project, you'll want a plain Ubuntu Linux image. There are two settings to make here. First, choose "OS Only" (rather than "Apps + OS"). Second, choose Ubuntu as the operating system.
4. #### Choose your instance plan.
	The instance plan controls how powerful of a server you get. It also controls how much money they want to charge you. For this project, the lowest tier of instance is just fine. And as long as you complete the project within a month and shut your instance down, the price will be zero.
5. #### Give your instance a hostname
	Every instance needs a unique hostname. You can use any name you like, as long as it doesn't have spaces or unusual characters in it. Your instance's name will be visible to you and to the project reviewer.
6. #### Wait for it to start up.
	It may take a few minutes for your instance to start up.
7. #### It's running
	Once your instance has started up, you can log into it with SSH from your browser.

	The public IP address of the instance is displayed along with its name. For intstance 54.84.49.254.

	Note: When you set up OAuth for your application, you will need a DNS name that refers to your instance's IP address. You can use the xip.io service to get one; this is a public service offered for free by Basecamp. For instance, the DNS name 54.84.49.254.xip.io refers to the server above.
	
	Explore the other tabs of this user interface to find the Lightsail firewall and other settings. You'll need to configure the Lightsail firewall as one step of the project.

	When you SSH in, you'll be logged as the ubuntu user. When you want to execute commands as root, you'll need to use the sudo command to do it.

## Configuring the AWS lightsail

1. ##### Updating And Managing software

```
sudo apt-get update
```

```
sudo apt-get upgrade
```

```
sudo apt-get autremove
```

2. ##### Configuring Uncomplicated Firewall (UFW)

```
sudo ufw status
```

## Deny all incoming threw firewall
```
sudo ufw default deny incoming
```

## Allow all outgoing threw firewall
```
sudo ufw default allow outgoing
```

## Allow ssh
```
sudo ufw allow ssh
```

## Allow ssh on port 2200 with tcp
```
sudo ufw allow 2200/tcp
```

## Allow for basic http server
```
sudo ufw allow www
```

## Allow NTP
```
sudo ufw allow ntp
```

## Allow ntp on port 123
```
sudo ufw allow 123/udp
```

## Enable Firewall
```
sudo ufw enable
```

## See the status of the firewall
```
sudo ufw status
```

## Secure your server
##### Disable root and create user that has ability to use sudo. Eliminates a attack vector threw remote login. Some services already create an user with these capabilities. If so you can skip this step.

1. #### Create user Grader for review sudo permission.
	#### Create new user grader
	```
	sudo adduser grader
	```
	#### Pass: grader
2. #### Changing to grader terminal.
	```
	sudo su grader
	```
	```
	cd
	mkdir .ssh
	chmod 700 .ssh
	```
	```
	cd
	touch .ssh/authorized_keys
	chmod 600 .ssh/authorized_keys
	```
3. #### Switching back to Ubuntu terminal(can close and reopen window terminal(since grader hasnt the apropriate permissions yet))
	```
	sudo cat /etc/sudoers
	```
	```
	sudo touch /etc/sudoers.d/grader
	```
	```
	sudo nano /etc/sudoers.d/grader
	```
	WRITE:
	grader ALL=(ALL) NOPASSWD:ALL
	COMMAND:
	ctrl^x
	Y
	ENTER
4. #### Creating key pair

	#### If you switch back to grader terminal it will have pseudo access!
	
	As local user:
	```
	ssh-keygen
	```
	Location for file should be:
	/home/YOUR_NAME/.ssh/PLACE_HOLDER
	Pass:catalog (for example)

	```
	cd
	sudo cat .ssh/PLACE_HOLDER.pub
	```
	As grader user:
	```
	cd 
	cd .ssh
	sudo nano authorized_keys
	```
	#### COPY the ssh-rsa key to authorized keys on ONE LINE.
	#### Follow the instructions to help copy and paste between terminals.
	##### To paste text into the browser-based SSH client

	1. Highlight text in your local desktop, then press Ctrl+C or Cmd+C to copy it to your local clipboard.

	2. In the bottom right corner of the browser-based SSH client, choose the clipboard icon. The browser-based SSH client clipboard text box appears.

	3. Click into the text box, then press Ctrl+V or Cmd+V to paste the contents from your local clipboard into the browser-based SSH client clipboard.

	4. Right-click any area on the SSH terminal screen to paste the text from the browser-based SSH client clipboard to the terminal screen.

	##### To copy text from the browser-based SSH client

	1. Highlight text on the terminal screen.

	2. In the bottom right corner of the browser-based SSH client, choose the clipboard icon. The browser-based SSH client clipboard text box appears.

	3. Highlight the text that you want to copy, then press Ctrl+C or Cmd+C to copy the text to your local clipboard. You can now paste the copied text anywhere in your local desktop.
	#### If reference needed see first link in WebBio

5. #### Change the port to 2200

	```
	sudo nano /etc/ssh/sshd_config
	```
	Locate "Port 22" in that file. Change it to "Port 2200" (At the moment i append it).
	```
	Restart ssh service.
	```
	For effect to take place the ssh service needs to restart
	```
	sudo service ssh restart
	```
	#### In the Networking tab of your instance you must add this custom port for it to be allowed on lightsail.
	See more info at [here](https://lightsail.aws.amazon.com/ls/webapp/eu-west-3/instances/catalog-Udacity/networking)
5. #### Disable base logins and forcing key-based logins
	```
	sudo nano /etc/ssh/sshd_config
	```
	Look for PasswordAuthentication if set to yes change it to no and save.
6. #### Restart ssh services.
```
sudo service ssh restart
```
7. #### to login from local machine use:
```
ssh grader@YOUR_PUBLIC_IP -p 22 -i ~/.ssh/PLACE_HOLDER
```
8. #### Switch to UTC
To switch to UTC, simply execute:
```
sudo dpkg-reconfigure tzdata
```
Scroll to the bottom of the Continents list and select Etc or None of the above; in the second list, select UTC. If you prefer GMT instead of UTC, it's just above UTC in that list. :)
9. #### Python using Apache and mod_wsgi install:

##### Python 2.x.x
```
sudo apt-get install python
sudo apt-get install python-setuptools
sudo apt-get install apache2 libapache2-mod-wsgi
```

##### Python 3.x.x
```
sudo apt-get install python3
sudo apt-get install python3-setuptools
sudo apt-get install apache2 libapache2-mod-wsgi-py3
```

#### Then start apache
```
sudo service apache2 start
```


10. #### Install PostgreSQL
```
sudo apt-get install postgresql
```

#### Don´t allow remote connections to PostgreSQL
https://www.digitalocean.com/community/tutorials/how-to-secure-postgresql-on-an-ubuntu-vps
```
sudo psql --version
```
#### The result of last command should be the input for the placeholder VERSION
(x.x)
```
sudo nano /etc/postgresql/VERSION/main/pg_hba.conf
```
Result should be close to:
##### Database administrative login by Unix domain socket
local   all 	postgres 	p$

##### TYPE  DATA BASE USER ADDRESS M$

##### "local" is for Unix domain socket connections only
local   all 	all 	p$
##### IPv4 local connections:
host    all 	all 	127.0.0.1/32 	m$
##### IPv6 local connections:
host    all 	all 	::1/128 	m$

--------------
#### Need to add the catalog user that will be created in the next step.

#### "local" privileges for catalog
local    all   catalog                p$

#### Add the above lines to pg_hba.conf after "local" is for Unix domain socket connections only BLOCK.
--------------

#### Reload postgresql
```
sudo service postgresql reload
```

#### To create catalog database, run the following to get into psql shell.
```
sudo -u postgres psql
```

#### Inside psql shell run:
```
create user catalog with password 'password123';
create database catalog with owner catalog;
```
For exit:
```
\q
```
#### Since the catalog has limited access to the database we won't be running:
#### Inside psql shell run:
```
grant all privileges on database <dbname> to <username>;
```
#### To enter as catalog user:
```
sudo -u postgres psql catalog
```
OR
```
psql postgresql://catalog:password123@loc
alhost/catalog
```
If you enter that means the database is correctly created.

#### Show all current tables in schema inside postgres
```
\dt
```
or
```
\d
```

11. #### Install git
```
sudo apt-get install git
```

12. #### To setup our server threw git.
Our project is at https://github.com/HugoSmits/catalogUdacityProject

To make sure that wsgi.py is not in the same folder as your project you can add a catalog dir before cloning
```
sudo mkdir catalog
```
```
cd /var/www/catalog
```
#### Create the wsgi.py

```
sudo nano catalog.wsgi
```
#### Contents of catalog.wsgi file.
```
#! /usr/bin/python

import sys
import logging
logging.basicConfig(stream=sys.stderr)

sys.path.insert(0,'/var/www/catalog/')
sys.path.insert(0,'/var/www/catalog/__init__.py')

from catalog import app as application

application.secret_key = 'SuperSecretKey'

```
the import can not be removed to avoid ImportError because it result in Error: Does not contain WSGI application 'application'.

##### To clone to the server to the catalog directory:
```
cd /var/www/catalog
sudo git clone https://github.com/HugoSmits/catalogUdacityProject catalog
```

##### If your project file has a different name than __init__.py a name change is needed for the wsgi.py file.
When using python 2 there must be a file named __init__.py

```
cd /var/www/catalog/catalog
sudo mv YOURNAMEFILE.py __init.py
```
##### Furthermore if your project has anything different that this:
```
app.run()
```
Substitute it in the main function by the code above.
To avoid this [error](https://stackoverflow.com/questions/17780291/python-socket-error-errno-98-address-already-in-use).

##### Then you can proceeed with the rest of the installation.
```
sudo apt-get install libapache2-mod-wsgi
sudo apt-get install libapache2-mod-wsgi python-dev
sudo a2enmod wsgi # enable wsgi
```
##### If needed to reread wsgi file. You can disable it and enable it.
```
sudo a2enmod wsgi # enable wsgi
sudo a2dismod wsgi # disable wsgi
```

#### For python 2.x.x
```
# install pip
sudo easy_install pip
# install requirements
cd catalog
sudo pip install -r requirements.txt
sudo python database_setup.py
sudo python lotsofmenus1.py
```

##### For python 3.x.x
```
# install pip3
sudo easy_install pip3
# install requirements
cd catalog
sudo pip3 install -r requirements.txt
sudo python database_setup_migrate.py db init
sudo python database_setup_migrate.py db upgrade
```

13. #### Now we need to run the project using Apache and mod-wsgi. So we will first create a configuration file for your project.

```
sudo nano /etc/apache2/sites-available/catalog.conf
```

```
<VirtualHost *:80>
	DocumentRoot "/var/www/catalog/"
	ServerName 35.180.121.218
	ServerAdmin ubuntu@35.180.121.218
	ServerAlias flaskcatalog.com
	WSGIScriptReloading On
	WSGIDaemonProcess catalog python-path=/var/www/catalog:/var/www/catalog/venv/lib/python2.7/site-packages
	WSGIProcessGroup catalog

	WSGIScriptAlias / /var/www/catalog/catalog.wsgi
	<Directory /var/www/catalog/catalog/>
		Order allow,deny
		Allow from all
	</Directory>

	Alias /static /var/www/catalog/catalog/static

	<Directory /var/www/catalog/catalog/static/>
		Order allow,deny
		Allow from all
	</Directory>

	ErrorLog ${APACHE_LOG_DIR}/error.log
	LogLevel warn
	CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
#### For automatic reloading of wsgi add:
WSGIScriptReloading On
(Hasnt worked yet...)

#### Disable default site.
```
sudo a2dissite 000-default.conf
sudo a2dissite 000-default
```

#### Enable the site.
```
sudo a2ensite catalog
```
#### Restart Apache
```
sudo service apache2 restart
```

#### The site can be reached at:
The server should be live now. Visit(htttp://35.180.121.218) !

-----------------------------

#### If service needs to restart
```
sudo a2dissite catalog
sudo service apache2 reload
sudo a2ensite catalog
sudo service apache2 reload
sudo service apache2 restart
```
#### If service pstgresql needs to restart
```
sudo service postgresql restart
sudo service postgresql status # Check status
```
#### Virtual Environment:
```
sudo virtualenv catalogenv
source catalogenv/bin/activate
```
```
deactivate #if succesful
```

----------------------

### If errors occur such as this one:

#### For error log enter:
Clear log before visiting:
```
sudo bash -c 'echo > /var/log/apache2/error.log'
```
```
sudo service apache2 restart
```
```
sudo cat /var/log/apache2/error.log
```

Check Apache Configuration SyntaxPermalink
Apache includes a nice little syntax checking tool. Use it to make sure you aren’t missing any brackets in your configuration files (and similar problems).

Debian and Ubuntu:
```
apache2ctl -t
```


#### IF:Failed to start LSB: Apache2 web server
```
sudo service apache2 status
```
```
apachectl stop
```

#### IF:Invalid command wsgiscriptalias perhaps misspelled or defined by a module not included in the server configuration.

Try to enable wsgi mod in Apache
```
sudo a2enmod wsgi
```
##### If you come across below error

ERROR: Module mod-wsgi does not exist!

You will have to install mod wsgi as below. What you have to do is run the following commands,
```
sudo apt-get install libapache2-mod-wsgi
sudo a2enmod wsgi
sudo service apache2 restart
```
#### IF: AH00094: Command line: '/usr/sbin/apache2' , AH00491: caught SIGTERM, shutting down

#### IF: how-do-i-run-systemctl-restart-apache2-in-aws-ubuntu-when-it-requires-a-password
```
sudo systemctl restart apache2
or
sudo service apache2 restart
```
```
AH00491: caught SIGTERM, shutting down
[Sat Feb 02 17:57:12.939813 2019] [wsgi:warn] [pid 11845:tid 140578556196736] mod_wsgi: Compiled for Python/2.7.11.
[Sat Feb 02 17:57:12.939845 2019] [wsgi:warn] [pid 11845:tid 140578556196736] mod_wsgi: Runtime using Python/2.7.12.
```

```
[wsgi:crit] [pid 19599:tid 139701741475712] mod_wsgi (pid=19599): The mod_python module can not be use
d on conjunction with mod_wsgi 4.0+. Remove the mod_python module from the Apache configuration.
AH00016: Configuration Failed
```
When i installed:
```
sudo apt-get remove libapache2-mod-python

sudo apt-get install libapache2-mod-python
```

I had many pid of apache2 running. Killing the excess and starting apache2 helped. Geting a cleaner status report.
https://askubuntu.com/questions/431925/how-to-restart-apache2-when-i-get-a-pid-conflict

```
tail -f /var/log/apache2/error.log
```


##### CHANGED THE VIRTUAL HOST TO WHAT IN THIS PAGE IS:
https://modwsgi.readthedocs.io/en/develop/user-guides/configuration-guidelines.html

----------------
##### THE PROBLEM LIES NOW IN WSGI SINCE I IMPORT A MODULE ON LINE 6 THAT DOESNT EXIST

## Contribuition

##### To contribuite to this project the person must follow the guidelines mentioned in **Usage** and send a written email to the moderator of the source code hugo.smits1995@gmail.com 

## Licensing info

##### The following project uses the [MIT license](https://opensource.org/licenses/MIT)

## WebBio

- ##### Full Stack Web Developer Nanodegree Udacity Lesson 5 From Linux Server Configuration -  "Get Started on LightSail"
- ##### https://lightsail.aws.amazon.com/ls/docs/en/articles/lightsail-how-to-connect-to-your-instance-virtual-private-server
- ##### https://aws.amazon.com/pt/premiumsupport/knowledge-center/new-user-accounts-linux-instance/
- ##### https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html
- ##### https://askubuntu.com/questions/138423/how-do-i-change-my-timezone-to-utc-gmt
- ##### https://stackoverflow.com/questions/13733719/which-version-of-postgresql-am-i-running
- ##### https://medium.com/coding-blocks/creating-user-database-and-adding-access-on-postgresql-8bfcd2f4a91e
- ##### https://serverfault.com/questions/268922/got-error-while-creating-database-in-postgres

- #### Apache Setup and Issues
- ##### https://askubuntu.com/questions/837372/apache2-service-is-not-active-cannot-reload
- ##### https://serverfault.com/questions/607873/apache-is-ok-but-what-is-this-in-error-log-mpm-preforknotice
- ##### https://stackoverflow.com/questions/20627327/invalid-command-wsgiscriptalias-perhaps-misspelled-or-defined-by-a-module-not
- ##### http://manpages.ubuntu.com/manpages/xenial/man8/a2enmod.8.html
- ##### https://askubuntu.com/questions/706128/ubuntu-clear-apache2-error-log
- #### Flask migration
- ##### https://flask-migrate.readthedocs.io/en/latest/
- ##### https://www.youtube.com/watch?v=BAOfjPuVby0
- ##### https://realpython.com/flask-by-example-part-2-postgres-sqlalchemy-and-alembic/
- ##### https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps
- ##### http://flask.pocoo.org/docs/1.0/deploying/mod_wsgi/
- #### Github
- ##### https://stackoverflow.com/questions/2047465/how-can-i-delete-a-file-from-git-repo
- #### Linux
- ##### https://www.computerhope.com/issues/ch000798.htm
- #### Postgresql
- ##### https://stackoverflow.com/questions/769683/show-tables-in-postgresql
- ##### https://knowledge.udacity.com/questions/26808
- #### Apache Debugging
- ##### https://serverfault.com/questions/607873/apache-is-ok-but-what-is-this-in-error-log-mpm-preforknotice
- ##### https://askubuntu.com/questions/431925/how-to-restart-apache2-when-i-get-a-pid-conflict
- ##### https://www.linode.com/docs/troubleshooting/troubleshooting-common-apache-issues/
- ##### https://www.digitalocean.com/community/tutorials/how-to-configure-logging-and-log-rotation-in-apache-on-an-ubuntu-vps
- ##### https://stackoverflow.com/questions/39929258/connection-refused-with-postgresql-using-psycopg2