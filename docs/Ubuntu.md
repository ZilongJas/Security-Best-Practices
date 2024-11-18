### Installs for Ubuntu
- `apt remove --purge 'php*'`
- `apt remove --purge 'libapache2-mod-php*'`
  - removes prevous installed PHP to prevent some errors (and reinstall them fresh)
- `sudo apt install apache2 mysql-server mysql-client php libapache2-mod-php`
  - installs the nesscary tools (LAMP)
- `systemctl enable --now apache2.service`
  - enables the service if not already enabled
- in a web browser type in http://localhost to test if it works
___
### Configurations
- `/etc/apache2/apache2.conf`
  - main config
- `/etc/apache2/conf-enabled/*.conf`
- `/etc/apache2/sites-enabled/*.conf`
- restart `apache2.service` whenever you make a change
- inside `/etc/apache2/conf-enabled` are symlinks to `/etc/apache2/conf-available`
  - disable symlinks using `sudo a2disconf [file]` without the "." after the file name. EX: `sudo a2disconf charset`
  - enable symlinks `sudo a2enconf [file]`
- why symlinks? So it's easier to enable/disable without deleting config files
___
### VirtualHost: host multiple websites in the same server
- `/etc/apache2/sites-enabled/000-default.conf`
  - default virtualhost
  - so if you want to create a new virtualhost, just copy that file to something like `001-example.com.conf`
  - note: the numbers is important, since you want the default virtualhost to be first hence the "000"
- `envvars` inside /etc/apache2 is a bash script that sets environment variables that Apache uses when it starts, also used to define paths, user/group information, or custsom variables
- and of course remember to restart apache2.service to apply changes
___
### PHP
- by default it is configured as an Apache module, meaning PHP is with Apache (different from RHEL!)
- `cd /var/www/html/`
  - `sudo nano phpinfo.php`
  - visit `http://[hostname].local/phpinfo.php`
  - if it does not work, you need to chmod some permissions, and enable services such as `a2enmod php[version#]`
___
### MySQL
- `systemctl status mysql`
- `sudo systemctl enable --now mysql`
  - usually ubuntu automatically enables it, so might not need this step
- `sudo mysql -u root`
  - login as root in mysql
___
### Setting up MySQL
- `USE mysql`
  - selects a database to work with, must be inside the mysql server shell already
- `CREATE USER 'admin'@'localhost' IDENTIFIED BY 'password';`
  - creates an admin user for our database (change your password)
- `GRANT ALL PRIVILEGES ON *.* TO 'admin'@'localhost' WITH GRANT OPTION;`
  - grants privileges
- `FLUSH PRIVILEGES;`
  - refreshes and applies the changes into memory
- `quit` to exit root and then login as the admin we just created: `mysql -u admin -p`
  - TIP! `system clear;` to clear screen
___
### Installing phpmyadmin
- `sudo apt install phpmyadmin` (during config, remember to use space to select options, a * will appear if selected)
- config file located in `/etc/apache2/conf-enabled/phpmyadmin.conf`
- `sudo systemctl restart apache2.service` restarts the service
- now go to `http://[hostname].local/phpmyadmin`
- add `Require local` inside the config file. So the website can only be accessed locally on your machine. Just below <Directory /usr/share/phpmyadmin>
- in phpmyadmin, add an user account
  - set hostname to `local`
  - check the box `Create database with same name and grant all privileges`
___
### WordPress
- https://wordpress.org download and extract
- on ubuntu go to `/var/www/html` delete everything inside and copy the wordpress files into here
- setup wordpress, use the same password as phpmyadmin. Follow instructions on the setup site
- after install, setup a password but do not use the same password this time
- in wordpress settings, general. Change wordpress address and site address url to [something.local] to avoid conflicts, incase multiple local projects are hosted on the same machine
___
### .htaccess file
- create one inside `/var/www/html`
- `sudo chmod www-data:www-data .htaccess`
- inside `/etc/apache2/sites-enabled/000-default.conf` at the bottom enter

        <Directory /var/www/html>
                AllowOverride ALL
         </Directory>
  
- restart apache.service
- used for URL rewriting, redirects, access control, custom error pages, password protection, caching etc...
___ 
### Protect a Directory with a Password
- `sudo htpasswd -c [file] [user]`
- filename: `.htpasswd`
- in the same folder as `.htaccess` file
- `sudo chown www-data:www-data .htpasswd`
  - make the webserver user the owner of this file (do this for other webserver files if things are not working)
- inside .htacess file for auth:
  
      AuthName "Login required!"
      Require valid-user
      AuthUserFile /var/www/html/.htpasswd
___
### SSH Tunnel
- enter on your main machine (not the server) `ssh -L 8088:localhost:80 username@[hostname].local -p 222`
  - forwards our port 8088 through ssh
  - going `localhost:8088/phpmyadmin` will now work through a ssh tunnel BUT only if you are connected through the ssh command already. Existing the ssh connection will close the connection



