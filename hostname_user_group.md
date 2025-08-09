# HOSTNAME
**Hostname** - a computer name in the net.

```
hostname
sudo hostname <new hostname> // temporary change
```

Change hostname in the file **/etc/hostname**
and in the file **/etc/hosts**
(need reboot)

# USER

```bash
sudo useradd [options] USERNAME #create new user
	-e --expitedate EXPIRE_DATE #YYYY-MM-DD
	-G --groups GROUPS
	-N --no-user-group

sudo usermod [options] USERNAME # edit user
	-a --append
	-d --home HOME_DIR
	-e --expiredate EXPIRE_DATE
	-g --gid GROUP #main group
	-G --groups GROUPS #addition groups
	-l --login NEW_LOGIN
	-L --lock
	-p --password PASSWORD #no crypt
	-r --remove #from addition groups
	-u --uid UID

sudo userdel [options] USERNAME
	-f --force
	-r --remove #delete HOME_DIR
```

## User info
```bash
id USERNAME
who
w #work
```

# GROUP
/etc/group

```bash
groups USERNAME #list of user groups

groupadd [options] GROUP # add new group
	-g -gid GID
	-p --password PASSWORD
	-U --user USERNAMES

groupmod [options] GROUP # edit group
	-a --append
	-g --gid GID
	-n --new-name NEW_NAME
	-p --password PASSWORD
	-U --users USERNAMES

groupdel [options] GROUP
	-f --force
```




