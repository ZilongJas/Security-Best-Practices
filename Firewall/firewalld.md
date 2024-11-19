### Overview of firewalld
- need to change ssh port back to the default 22 for seamless connection when configuring over ssh. Check [revert_port_change_rhel.md](https://github.com/ZilongJas/SSH_Best_Practices/blob/main/revert_port_change_rhel.md) 
___
### Inspecting our system for opened ports
- IPv4:
```bash
sudo ss -4nap
```
- IPv6
```bash
sudo ss -6nap
```
- `ss`: The socket statistics command, used to view information about sockets.
- `4`: Filters to show only IPv4 sockets.
- `n`: Displays numerical addresses instead of resolving hostnames.
- `a`: Shows all sockets, including listening and non-listening sockets.
- `p`: Displays the process using each socket, including the process ID (PID) and name
___
### Architecture of firewalld
- Linux kernel: netfilter
- Firewalld backends: nftables
- Daemon and service: firewalld
- Tools: firewall-cmd, firewall-config
___
### By default: firewalld
- By default all incoming TCP and UDP connections will be blocked except the ones that are allowed
- ssh (port 22) default enabled
- dhcpv6-client (udp port 546) default enabled
- IPv6 is enabled by default
- Certain ICMP requests are blocked to prevent common network attacks (Ping, traceroute)
___
### Check current firewalld config
```bash
sudo firewall-cmd --state
```
```bash
sudo firewall-cmd --list-all
```
___
### Ubuntu
- on Ubuntu, we have ufw (uncomplicated firewall). We want to install firewalld instead because it is more advanced
```bash
sudo ufw disable
```
```bash
sudo apt install firewalld
```
```bash
sudo systemctl enable --now firewalld
```
___
### RHEL
```bash
/etc/firewalld/
```
services folder on RHEL, but avoid editing files directly
```bash
sudo firewall-cmd --help
```
EXAMPLE:
```bash
sudo firewall-cmd --permanent --service=[service] --add-port=[port]/tcp
```
___
### more on firewalld
```bash
sudo firewall-cmd --get-services
```
for services
```bash
sudo firewall-cmd --info-service [service]
```
for info on the service













