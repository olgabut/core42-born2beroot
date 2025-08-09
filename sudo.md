## sudo (superuser do)
Allows authorized users to run comands with the security privileges of another user (root)

## su (switch user)
```bash
su - #switch to root
su <USERNAME> #switch to USERNAME
```

# Privilege separation
**The sudoers policy** module determines a user's sudo privileges.
It is the default sudo policy plugin.
The policy is driven by the **/etc/sudoers** file or, optionally in LDAP (Lightweight Directory Access Protocol).

```bash
sudo -V # info
```

In the file **/etc/sudoers** but use following command to change this file safely
```
sudo visudo
```

Change file

```bash
USERHOST = (RUN-AS-USER : RUN-AS-GROUP) COMMANDS
Defaults  passwd_tries=3
Defaults  badpass_message="Ah jo! Password is wrong! Please try again. "

Defaults  logfile="/var/log/sudo/sudo.log"
Defaults  log_input, log_output
Defaults  iolog_dir="/var/log/sudo"

Defaults  requiretty #TTY (Teletypewriter)

Defaults  secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"
```

```bash
sudo mkdir -p /var/log/sudo # create folder
sudo touch /var/log/sudo/sudo.log # create file
```