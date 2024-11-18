### Installs for RHEL
- `sudo dnf install httpd mysql mysql-server php`
  - Installs Apache, MySQL, & PHP
- `systemctl enable --now httpd.service`
  - enables and lauches httpd.service
  - in a web browser type in http://localhost to test if it works
___
### Configurations
- `/etc/httpd/conf/httpd.conf`
  - main config
- `/etc/httpd/conf.modules.d/*.conf`
- `/etc/httpd/conf.d/*.conf`
- restart `httpd.service` whenever you make a change
- `sudo journalctl -u httpd.service` to get the logs
___
### Changing ports
- in the main config under "Listen" enter `Listen [port #]`
___
### VirtualHost: hprivileges or has restrictions preventing login from localhost multiple websites on the same server
- Inherit all configs in the main config file
- create a conf file in `/etc/httpd/conf.d` called `[hostname].local.conf`
  - ex: [rocky.local.conf](/docs/rocky.local.conf)
- `sudo mkdir /var/www/html/rocky.local`
- `sudo nano /var/www/html/rocky.local/index.html`
  - makes a start page
- `sudo restorecon -R /var/www/html/`
  - refresh the file context incase some files are missing/incorrect etc...
- remember to restart httpd.service when changing configs
- inside `/etc/httpd/conf.d` copy [rocky.local.conf](/docs/rocky.local.conf) to a new file called [000-default.conf](/docs/000-default.conf)
  - `sudo mkdir /var/www/html/localhost`
  - `sudo dnf install nss-mdns` (allows hostname for .local domains)
  - `sudo systemctl restart httpd`
  - reboot
  - why? so the apache serves a default site (localhost) if no specific virtual host matches the request. aka a landing page for unmatched or mistyped domains.
- SUMMARY:
  - virtualhost htmls are localed in `/var/www/html/`
  - virtualhost configurations are localted in `/etc/httpd/conf.d/`
___
### Extras
- `sudo systemctl stop firewalld`
  - disables the firewall when testing incase it is blocking network traffic
- logs: EXAMPLES
  - error log: when permissive files gets denied.
  - custom log: whenever the website is accessed (almost logs everything)
  - use `tail -f [log]` to follow the logs in real time
___
### PHP
- configured through php-fpm, meaning it's a separate service just for running PHP (different on ubuntu!)
- `cd /var/www/html/rocky.local/`
  - `sudo nano phpinfo.php`
  - creates a php file, visit `http://[hostname].local/phpinfo.php`
___
### MySQL
- `systemctl status mysqld`
- `sudo systemctl enable --now mysqld`
  - enables mysqld
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
- `sudo dnf install phpmyadmin`
- go to `phpMyAdmin.conf` in `/etc/httpd/conf.d`
- visit `http://[localname].local/phpmyadmin`
  - only works locally
- disable firewall if it's blocking you from doing this `sudo systemctl stop firewalld`
___
*Everything below here MIGHT be different on RHEL, so beware
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





