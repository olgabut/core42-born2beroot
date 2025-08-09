# Documentation
https://www.w3schools.com/bash/index.php

# How to check if the settings are done

**Encrypted partitions using LVM**

- [] LVM
```bash
lsblk
```
*The result should look like the task screenshot (3 or 7 encrypted sections)*

---

**User checks**

<input type="checkbox" /> Check for the user presence
```bash
cat /etc/passwd | awk -F":"'{print $1}'
```
*Find user with login in intra-42 system (user).*

<input type="checkbox" /> The user has groups "sudo", "user42"
```bash
groups
```
*Should be sudo and user42 in the list.*

<input type="checkbox" /> User policy
```bash
nano /etc/sudoers #(sudo visudo)
#Should be
<user> ALL=(ALL:ALL) ALL
Defaults    passwd_tries = 3
Defaults    badpass_message = "Password is wrong. Please try again."
Defaults    logfile="/var/log/sudo/sudo.log"
Defaults    log_input,log_output
Defaults    iolog_dir="/var/log/sudo"
Defaults    requiretty
Defaults    secure_path="/usr/local/sbin:usr/local/bin:/usr/sbin:usr/bin:/sbin:/bin:/snap/bin"
```
```bash
sudo -k  # reset password cache
sudo ls  # try entering the wrong password 3 times
sudo tail /var/log/sudo/sudo.log
```

<input type="checkbox" /> Hostname
```bash
nano /etc/hostname
nano /etc/hosts
```
*Should be the correct host name.*

---

**SSH checks**

<input type="checkbox" /> SSH config
```bash
nano /etc/ssh/sshd_config
#Should be in the file
Port 4242
PermitRootLogin no
PasswordAuthentication yes
```

<input type="checkbox" /> Connect
```bash
ssh <user>@localhost -p 4242
```

---

**UFW**

<input type="checkbox" /> Firewall
```bash
sudo ufw status
#Should be
4242 ALLOW
4242 (v6) ALLOW
```

---

**Security modules for the Linux kernel**

<input type="checkbox" /> AppArmor
```bash
sudo aa-status
nano file /etc/default/grub
#Should be
GRUB_CMDLINE_LINUX="apparmor=1 security=apparmor"
```

---

**Password policy**

<input type="checkbox" /> Password policy

```bash
#for existing users
sudo chage -M 30 -m 2 -W 7 <user>
#for new users
nano /etc/login.defs
#Should by
PASS_MAX_DAYS 30
PASS_MIN_DAYS 2
PASS_WARN_AGE 7
```

**PAM - pluggable authentication module**

<input type="checkbox" /> libpam-pwquality file pwquality.conf

```bash
passpwd <username>
nano /etc/security/pwquality.conf
#Sould be
minlen = 10
dcredit = -1
ucredit = -1
lcredit = -1
minclass = 3
maxrepear = 3
usercheck = 1
enforce_for_root
```

<input type="checkbox" /> libpam-pwquality file pam.d/common-password

```bash
nano /etc/pam.d/common-password
#Should be
password        requisite                       pam_pwquality.so retry=3 difok=7
password        [success=1 default=ignore]      pam_unix.so obscure use_authtok try_first_pass yescrypt
password        requisite                       pam_deny.so
password        required                        pam_permit.so
```

**Script monitoring.sh**

<input type="checkbox" /> Cron