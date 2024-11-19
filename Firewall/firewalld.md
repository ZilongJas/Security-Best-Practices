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
