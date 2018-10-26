# Linux-Server-Configuration



## Project Description

Linux Server Configuration is the sixth project for **Udacity Full Stack Developer Nanodegree** this project created by Youssef Essam.
Course <a href="https://eg.udacity.com/course/full-stack-web-developer-nanodegree--nd004">Link</a>

* IP address: 35.178.8.18

* SSH port: 2200

* Application URL: <a href = "http://ec2-35-178-8-18.eu-west-2.compute.amazonaws.com" >http://ec2-35-178-8-18.eu-west-2.compute.amazonaws.com</a>
## Amazon Lightsail

1. Go to the Amazon Lightsail website and Sign Up
2. <a href = "https://lightsail.aws.amazon.com/" target="_blank" >Sign in</a>
3. Create your first instance
4. Scroll down and go to account page and download key
5. After download rename the file to  udacity.pem and move it into C:\Users\[USERNAME]\.ssh
6. Open network and click add another chose Custom and in port range add 123
7. Repeat steap 6 and add anther port range 2200

## Linux Configuration
### open git bash and run this commands

```
$ vagrant init ubuntu/trusty64
$ vagrant up
$ chmod 600 ~/.ssh/udacity.pem
$ ssh -i ~/.ssh/udacity.pem ubuntu@YOUR PUBLIC ID
$ sudo su -
$ sudo adduser grader
$ sudo nano /etc/sudoers.d/grader
```
* And add this lines grader ALL=(ALL:ALL) ALL
```
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt-get dist-upgrade
$ sudo apt-get install finger
```
* And open new terminal and run this command 
```
$ ssh-keygen -f ~/.ssh/udacity.rsa
$ cat ~/.ssh/udacity.rsa.pub
```
* Copy the key from the terminal.
* Back in the server terminal locate the folder for the user grader
```
$ mkdir .ssh
$ touch .ssh/authorized_keys
$ nano .ssh/authorized_keys
```
* Now paste the public key to save 'click Ctrl+O press Enter then F2'
```
$ sudo chmod 700 /home/grader/.ssh
$ sudo chmod 644 /home/grader/.ssh/authorized_keys 
$ sudo chown -R grader:grader /home/grader/.ssh
$ sudo service ssh restart
```
* Disconnet from the server
```
$ ssh -i ~/.ssh/udacity.rsa grader@YOUR PUBLIC ID
$ sudo nano /etc/ssh/sshd_config
```
* Then
1. change port 22 to 2200
2. PasswordAuthentication to no
3. PermitRootLogin to no

```
$ sudo service ssh restart
$ sudo ufw allow 2200/tcp
$ sudo ufw allow 80/tcp
$ sudo ufw allow 123/udp
$ sudo ufw enable
$ sudo ufw status
$ sudo apt-get install apache2
$ sudo apt-get install libapache2-mod-wsgi python-dev
$ sudo apt-get install git
$ sudo a2enmod wsgi
$ sudo service apache2 restart
$ cd /var/www
$ sudo mkdir catalog
$ sudo chown -R grader:grader catalog
$ cd catalog
$ git clone REPOSITORY LINK catalog
$ sudo nano catalog.wsgi
```
* Paste This Code
```
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0, "/var/www/catalog/")

from catalog import app as application
application.secret_key = 'super_secret_key'
```
* Rename your application.py or whatever you called it in your catalog application folder to ```__init__.py``` by 
``` 
$ mv project.py __init__.py 
```

```
$ sudo pip install virtualenv
$ sudo virtualenv venv
$ source venv/bin/activate
$ sudo chmod -R 777 venv
$ sudo apt-get install python-pip
$ sudo pip install flask
$ sudo pip install httplib2 oauth2client sqlalchemy psycopg2
```

* Now open ```__init__.py``` and Change any client_secrets.json path to /var/www/catalog/catalog/client_secrets.json
```
$ sudo nano /etc/apache2/sites-available/catalog.conf
```
* Paste this code:
```
<VirtualHost *:80>
    ServerName 35.167.27.204
    ServerAlias ec2-35-167-27-204.us-west-2.compute.amazonaws.com
    ServerAdmin admin@35.167.27.204
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
* continue on your terminal
```
$ sudo a2ensite catalog.conf
$ a2dissite 000-default.conf
$ sudo apt-get install libpq-dev python-dev
$ sudo apt-get install postgresql postgresql-contrib
$ sudo su - postgres -i
$ psql
```
* Create a database user and password
```
postgres=# CREATE USER catalog WITH PASSWORD [password];
postgres=# ALTER USER catalog CREATEDB;
postgres=# CREATE DATABASE catalog with OWNER catalog;
postgres=# \c catalog
catalog=# REVOKE ALL ON SCHEMA public FROM public;
catalog=# GRANT ALL ON SCHEMA public TO catalog;
catalog=# \q
$ exit
```
* Now in ``` __init__.py ``` and database_setup.py and lotsofitems.py Change ``` sqlite://catalog.db ``` To ``` postgresql://username:password@localhost/catalog ```

* Then continue work on your terminal
```
$ sudo service apache2 restart
```
## Update the Google OAuth client secrets file 
1. Fill in the client_id and client_secret fields in the file client_secrets.json.
2. change the javascript_origins field to  "http://35.178.8.18", "http://ec2-35-178-8-18.eu-west-2.compute.amazonaws.com"
3. Change authorized redirect URIs to "http://ec2-35-178-8-18.eu-west-2.compute.amazonaws.com/oauth2callback"

These addresses also need to be entered into the Google Developers Console -> API Manager -> Credentials, in the web client under "Authorized JavaScript origins".
## Contact Me

* **Youssef Essam**
* Egypt, Cairo
* <a href ="https://www.facebook.com/yossef.essam.1213">Facebook</a>
* youssef.3ssam1@gmail.com
* +201116008332
