### SELinux 
- mainly for RHEL distros
- ubuntu runs apparmor
- developed by NSA, now open source
- help prevent all sorts of attacks
- Discretionary access control, which focuses its permissions on groups and users
- Mandatory access control focuses on rules that grant permissions to specific processes

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
chcon -t user_home_t -R -v [folder]
```
- meant for temporary debugging

```bash
sudo restorecon [file]
```
defaults to SELinux policy

```bash
semanage fcontext -l
```
lists all the file context mappings

1. Add a New Rule

        semanage fcontext -a -t httpd_sys_content_t '/public(/.*)?'

    Purpose: Defines that the /public directory and all its contents (/.*) should have the httpd_sys_content_t SELinux type.
   
    Why? To ensure that files in /public can be accessed by the web server because this type (httpd_sys_content_t) is specifically allowed by SELinux for web content.

3. Map to an Existing Context

        semanage fcontext -a -e /usr/share/nginx/html /public

    Purpose: Maps /public to use the same SELinux context as /usr/share/nginx/html without manually defining a new rule.
   
    Why? This is useful when you know another directory (e.g., the default web content directory) already works with SELinux policies, and you want to replicate its behavior for /public.

4. Apply the Rules to Files

          restorecon -R /public

    Purpose: Updates the SELinux labels of the actual files and directories under /public to match the newly defined rules.
   
    Why? Without this step, the SELinux rules are just definitions—they won’t take effect until applied.




