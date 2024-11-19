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

`semanage fcontext [parameters]`

- -l	List all existing file context rules.
- -a	Add a new file context rule.
- -d	Delete an existing file context rule.
- -e	Map one directory's context rules to another directory.
___
### semanage fcontext vs restorecon
- SELinux: `semanage fcontext` vs `restorecon`

 What They Do:

- **`semanage fcontext`**:
  - Defines **default context rules** for files or directories.
  - These are **persistent rules** that survive reboots.

- **`restorecon`**:
  - Applies the context rules defined by `semanage fcontext`.
  - Ensures file labels match defined rules.

## Analogy:
- **`semanage fcontext`** = Blueprint for how files should be labeled.
- **`restorecon`** = Worker that applies the labels using the blueprint.

## Example Workflow:

1. **Define a Rule**:
    ```bash
    semanage fcontext -a -t httpd_sys_content_t '/public(/.*)?'
    ```
    - Labels `/public` and all its files/directories as `httpd_sys_content_t`.

2. **Apply the Rule**:
    ```bash
    restorecon -R /public
    ```
    - Updates `/public` with the correct label based on the rule.

3. **Check Labels**:
    ```bash
    ls -Z /public
    ```
    - Shows the current SELinux labels.

## Important Note:
- Without defining rules using `semanage fcontext`, `restorecon` has nothing to apply, which may lead to incorrect labels and access issues.
___
### Processes
- use system monitor on a GUI
- or `sudo ps -Z` on a command line
- `sudo ps -efZ` for more
- using `htop` (must be installed, repo epel-release have to be enabled for RHEL), enable `SECATTR` (Security Attribute)
___
### Polices
- `.fc`
  - file context definitions
- `.te`
  - type enforcement rules, defines transitions
- `.if`
  - contains interfaces / functions that can be used in the .te file
___
### targeted policy
- `id -Z`
  - SELinux ID
- `sudo semanage login -l`
  - manage user access with SELinux policies
___
### Booleans
- simple way to configure our system (finally, i'm tired)
```bash
getsebool -a
```
available booleans

```bash
semanage boolean -l
```
explanations about them

- Temporarily:

         setsebool [boolean] on
         
- Permanently:

         setsebool -P [boolean] on
___
### debugging
- `/var/log/audit/audit.log`
  - recorded in here
```bash
ausearch -ts recent
```
- By default, ausearch would display all violations of today
- But with -ts recent, only the last 10 minutes will be shown

```bash
journalctl -t setroubleshoot --since=14:20
```
- Once you find an entry, you can query journalctl






















