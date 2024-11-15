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
### VirtualHost: serve multiple pages from the same httpd instance
- Inherit all configs in the main config file
- create a conf file in `/etc/httpd/conf.d` called `[hostname].local.conf` 
