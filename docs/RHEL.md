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
### VirtualHost: host multiple websites on the same server
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
  - virtualhosts are localed in `/var/www/html/`
  - configurations are localted in `/etc/httpd/conf.d/`
___
### Extras
- `sudo systemctl stop firewalld`
  - disables the firewall when testing incase it is blocking network traffic 
