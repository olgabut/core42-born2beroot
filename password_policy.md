# Set user password
```bash
passwd [options] USERNAME
	-d --delete
	-e --expire
	-l --lock
	-S --status
	-u --unlock
	-n --mindays MIN_DAYS # to change password
	-w --warndays WARN_DAYS # show warnings
	-x --maxdays MAX_DAYS
```

# Change user password expiration and litetime policies
```bash
chage [options] USERNAME
	-d --lastday LAST_DAY #last day password change
	-E --expiredate EXPIRE_DATE
	-i --inactive INACTIVE #block user, if user was inactive this days
	-l --list # information
	-m --mindays MINDAYS # min days to change password
	-M --maxdays MAXDAYS # max days to change password
	-W --wardays WARN_DAYS # show warnings
```

In the file **/etc/login.defs**
```bash
PASS_MAX_DAYS  30 # expite every 30 days
PASS_MIN_DAYS  2 # min days before the modification
PASS_WARN_AGE  7 # show warnings
```

# PAM (pluggable authentication module)
```bash
sudo apt install libpam-pwquality #install
```

Settings in the file **/etc/security/pwquality.conf**
```bash
minlen = 10
dcredit = -1 # digit
ucredit = -1 # uppercase
lcredit = -1 # lowercase
ocredit = -1 # other characters
minclass = 3
maxrepeat = 3
usercheck = 1 # no user name in password
enforcing = 1 # important rules, not just logs
difok = 7 # different chars between old and new passwords
enforce_for_root # for root user too
retry = 3
```

Settings in the file **/etc/pam.d/common-password**

```bash
password requisite pam_pwquality.so retry=3 defok=7
```