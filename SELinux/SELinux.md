### SELinux 
- mainly for RHEL distros
- ubuntu runs apparmor
- developed by NSA, now open source
- help prevent all sorts of attacks
___
### Enabling / Disabling SELinux
- `sudo getenforce` check if enforced
- `sestatus` to check status of SELinux
- `sudo setenforce 1` enforcing
- `sudo setenforce 0` permissive

```bash
sudo nano /etc/selinux/config
```
- Permanently change status

```bash
ls -Z
```
- `[SELinux user]:[SELinux role]:[SELinux type]:[SELinux level]`

```bash
chcon [-u USER] [-r ROLE] [-l RANGE] [-t TYPE] [-R] [-v] FILE
```
- -R: Recursive
- -v: verbose (print which files have been changed)
EX: 
```bash
chcon -t user_home_t -R -v folder
```
- meant for temporary debugging










