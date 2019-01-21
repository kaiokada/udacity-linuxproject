# udacity-linuxproject
Final Project - Linux Configuration - for Full-Stack Web Development Nanodegree

Author: Kai Okada

In this Udacity project, I generated an instance of Amazon Lightsail and used it to create an Ubuntu Linux server, per the project recommendations. I configured my server according to project specifications, and hosted my Udacity Item Catalog project on the server. Details for the configuration are as follows:

IP Address: 34.228.191.114
SSH Port: 2200 (nonstandard)
URL for Item Catalog Application: http://34.228.191.114.xip.io/catalog
(Per course recommendation, I used xip.io as a wildcard domain to allow Google to treat the URL as registered for OAuth)

Changes made to the default server configuration:
- Alter default SSH port from 22 to 2200. This was done via the "/etc/ssh/ssh_config" file.
- Enforce key-based authentication (Got keys using ssh-keygen on my local machine) and disallow remote login to root. These were also done via the "/etc/ssh/ssh_config" file ("PasswordAuthentication no", "PermitRootLogin no"). I referenced this article for the latter: https://mediatemple.net/community/products/dv/204643810/how-do-i-disable-ssh-login-for-the-root-user.
- Add test user 'kaiokada' and granted sudo access as a secondary log-in to server from my local machine (in addition to default 'ubuntu').
- Add user 'grader' and granted sudo access per project requirements.
- Set up UFW to allow only HTTP (80), NTP (123), and nonstandard SSH (2200) requests.
- Download PostgreSQL and created role + database named 'catalog'. Added corresponding user named 'catalog' to the server with access to the catalog psql database (later, needed to change the SQLAlchemy "create_engine" arguments from the previous sqlite3 database to a postgresql database that I logged into via the 'catalog@' user, password 'udacity' - I referenced this SQLAlchemy documentation: https://docs.sqlalchemy.org/en/latest/core/engines.html#postgresql). I needed to reference DitigalOcean in order to do this properly: https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-16-04.
- Download Apache2, git, and mod_wsgi per project specifications, and cloned GitHub Item Catalog Project directory to '/var/www/flask_app' on the server as 'catalog' directory. Renamed 'application.py' to '__init__.py' and created file myapp.wsgi as the application handler for the catalog. 
- Configure Apache2 via "/etc/apache2/sites-enabled/000-default.conf". Added "ServerName 34.228.191.114.xip.io", "ServerAdmin ubuntu@34.228.191.114.xip.io", "DocumentRoot /var/www/flask_app", "WSGIScriptAlias / /var/www/flask_app/catalog/myapp.wsgi."
- Configure timzone to UTC using "sudo dpkg-reconfigure --frontend noninteractive tzdata" per instructions here: https://help.ubuntu.com/community/UbuntuTime

Software Installed:
- apache2
- libapache2-mod-wsgi
- postgresql
- postgresql-contrib
- python-flask
- python-sqlalchemy
- python-urllib
- python-oauth2client
- python-httplib2
- python-requests

Please refer to the "Notes to the Reviewer" section for 'grader's SSH RSA Key.

