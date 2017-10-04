1) IP Address with Port Number: 107.21.53.243:2200
2) The complete URL to the hosted web application: http://107.21.53.243
3) The list of softwares I installed and configurations I made are as follows:
	a) Created an Amazon Instance named "cheese".
	b) Downloaded the default .pem private key for default 'ubuntu' user.
	c) Changed the permission of default .pem file.
	d) Updated and upgraded all the packages on lightsail instance.
	e) Logged in via default user and changed the ssh port from 22 to 2200.
	f) Made changes to the internal and external firewalls of the instance by adding 2200 (ssh), 80 (http) and 123 (ntp).
	g) Created a new user name "grader".
	h) Create a new file in sudoers.d directory named 'grader'.
	i) Gave it sudo privilages by adding following line in sudoers.d/grader: 'grader ALL=(ALL) NOPASSWD:ALL'.
	j) Generated public and private key pair by running 'ssh-keygen' command in local machine.
	k) Created the .ssh folder and authorized_keys file inside it.	
	l) Copied and pasted public key generated at local machine into the lightsail instance file 'authorized_keys' under '.ssh/' directory in single line.
	m) Changed the .ssh and authorized_keys permission to make it only being used by grader user.
	n) Logged into lightsail instance 'chees' as grader user by running following command in local terminal: ssh grader@107.21.53.243 -p 2200 -i ~/.ssh/lightsail/lightsail
	o) Checked the local timezone on lightsail instance using 'date' command and also changed it to UTC timezone by running following command: sudo dpkg-reconfigure tzdata
	p) Install the Apache to serve a Python wsgi application by running following command: sudo apt-get install apache2
	q) Install mod_wsgi using 'sudo apt-get install libapache2-mod-wsgi'.
	r) Install postgresql by 'sudo apt-get install postgresql postgresql-contrib' command.
	s) Checked the postgresql '/etc/postgresql/9.5/main/pg_hba.conf' file for any remote connections and remove them. By default, no remote connections were allowed but I needed to change the login method for local user from peer to 'md5'.
	t) Created a new database user by running following commands: 
		sudo -u postgres -i
		psql
		create user catalog with password 'catalog';
		alter role catalog login;
		alter role catalog CREATEDB;
		\q
		psql -U catalog catalog
		create database catalog with owner catalog;
		revoke all on database catalog from PUBLIC;
		grant all on database catalog to catalog;
		\q
		logout
		logout
	u) Git was already installed so, no need to install it again.
	v) Cloned the Item Catalog repo inside /var/www/ItemCatalog/ folder and change the finalproject.py to __init__.py using 'sudo mv finalproject.py __init__.py' command.
	w) Created a static folder in '/var/www/ItemCatalog/ItemCatalog/' directory, changed app.run(host='0.0.0.0', port=5000) to app.run() in __init__.py and changed the database engine from mysql to postgresql by writing 'postgresql+psycopg2://catalog:catalog@localhost:5432/catalog' in __init__.py, database_setup.py and lotsofplaces.py files.
	x) Created a ItemCatalog.conf in '/etc/apache2/sites-available' directory by running 'sudo nano /etc/apache2/sites-available/ItemCatalog.conf' and wrote the config file with proper paths to the project dirctory. Run 'sudo a2ensite ItemCatalog' to enable the virtual host and stop the default app by running 'sudo a2dissite 000-default.conf' command. Created a wsgi app in /var/www/ItemCatalog directory by 'sudo nano itemcatalog.wsgi' command and enabled logging in it. Restart the apache2 service by 'sudo service apache2 restart' command.
	y) Wrote the directory match tag in apache2.conf under '/etc/apache2/' directory  to make .git directory inaccessible from browser.
	z) Changed the relative path to absolute path of client_secrets.json and added the new client_secret.json after adding new IP address and instance URL into Authorized Javascript origins and Authorized Redirect URIs of Credential Tab in Google API Console.
4) Third party resources used in completing this project: https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps

