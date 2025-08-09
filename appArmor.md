# Security
**DAC (Discretionary Access Control)** is a security mechanism where the owner of a resource (like a file or folder) decides who has access to it and what level of access they have.
(read, write, eXecute)
(user owner, group, other users)

**MAC (Mandatory Access Control)** is a security mechanism where the operating system strictly enforces access based on predefined security policies, rather than individual user preferences.


**SELinux** and **AppArmor** are security modules for the Linux kernel. They provide **Mandatory Access Control (MAC)**, they help make your system more secure by controlling what programs can do.

## SELinux (Security-Enhanced Linux)

* Developed by the NSA (U.S. National Security Agency).
* Works based on security contexts and policies applied to every file, process, port, etc.
* Highly flexible and powerful, but complex to configure and troubleshoot.
* Commonly used in Red Hat, CentOS, Rocky Linux, and Fedora.

## AppArmor (Application Armor)

* Uses path-based profiles (like /usr/sbin/nginx) to define what a process can or cannot do.
* Easier to understand, configure, and maintain.
* Commonly used in Debian and Ubuntu.


### Comparison SELinux and AppArmor
| Feature                  | SELinux                | AppArmor              |
| ------------------------ | ---------------------- | --------------------- |
| Configuration complexity | High                   | Low to Medium         |
| Flexibility              | Very High              | Sufficient            |
| Access control model     | Label/context-based    | Path-based            |
| Commonly used in         | Red Hat, Rocky, Fedora | Debian, Ubuntu        |
| Kernel support           | Built into the kernel  | Built into the kernel |


### Work with AppArmor

#### Install
```
sudo apt update
sudo apt install apparmor apparmor-utils apparmor-profiles
```

#### Check
```
sudo aa-status
```

Result
```bash
apparmor module is loaded.
45 profiles are loaded.
5 profiles are in enforce mode. #Профили в enforce режиме активно ограничивают работу программ.
40 profiles are in complain mode. #Режим обучения: пишет предупреждения
2 processes have profiles defined. #Сколько работающих программ (процессов) сейчас используют AppArmor.
1 processes are in enforce mode.
1 processes are in complain mode.
```


### Run AppArmor on startup
in file /etc/default/grub   add line:
```
GRUB_CMDLINE_LINUX="apparmor=1 security=apparmor"
```

Update grub
```
sudo update-grub
```

### Active profile
```bash
sudo aa-status | grep sshd
sudo aa-enforce /etc/apparmor.d/usr.sbin.sshd # Включает защиту профиля
sudo aa-disable <профиль> # Выключает защиту профиля
sudo aa-complain <профиль> # Только пишет предупреждения
```
