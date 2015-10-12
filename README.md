# Udacity Linux Server Configuration #

## IP Address ##

52.89.3.21

SSH Port 2200

## Administration ##

Sudo password for _grader_: `graduda`

To access PostgreSQL database:
* `sudo -i -u catalog`
* `psql`

## Application URL ##

http://ec2-52-89-3-21.us-west-2.compute.amazonaws.com/

## Installed Software ##

This is the software installed and configured on the server, as well as
the command used to configure it.

* Apache Web Server: `sudo apt-get install apache2`
* Apache mod WSGI: `sudo apt-get install libapache2-mod-wsgi`
* PostgreSQL: `sudo apt-get install postgresql`
* Python PIP: `sudo apt-get install python pip`
* Python Flask: `sudo pip install flask`
* Python Flask Seasurf: `sudo pip install flask-seasurf`
* Python oauth2client: `sudo pip install oauth2client`
* Python psycopg2: `sudo pip install psycopg2`
* Python sqlalchemy: `sudo pip install sqlalchemy`

## Configuration Process ##

### Sudo ###
Created the _grader_ user and added it to the _sudo_ group so it could have
access to the sudo command.

### PostgreSQL ###
For PostgreSQL the following configuration was done:
* Created the role _catalog_, and the unix user _catalog_ to match it. This was 
done on the unix user _postgres_ that was created when PostgreSQL was installed.
* Created the database _catalog_, as default PostgreSQL gives full 
permissions to the database with the same name.
* Define a password for the role _catalog_. As the role were created using a 
unix account autenthication, it didn't have a password. We need to set one to be
able to access the user from within our application.
* Edited the file `/etc/postgresql/9./main/postgresql.conf` and uncommented the
line `listen_addresses='localhost'` to disallow remote connections.

### Apache ###
Edited the `/etc/apache2/sites-enabled/000-default.conf` file and added the 
line `WSGIScriptAlias / /var/www/html/myapp.wsgi` to allow the execution of the
python application.

### Application ###
Cloned the project to the home directory, and then created a symbolic link
from `/home/grader/catalog-udacity/vagrant/catalog` to `/var/www/html` to 
allow the application to run. Also did some modification to the project like 
changing the database from sqlite to PostgreSQL and adding the WSGI loader.

## Resources ##
I complemented the course information with the following guides:
* https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-14-04
* http://www.cyberciti.biz/tips/postgres-allow-remote-access-tcp-connection.html
