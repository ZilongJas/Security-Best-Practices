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
