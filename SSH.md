# SSH (Secure Shell)
SSH (Secure Shell) is a network protocol that provides a secure way to access and manage remote computers over an unsecured network. It's used for remote login, command execution, and file transfer.


## Install OpenSSH Server
```
sudo apt update
sudo apt install openssh-server
```

## Status SSH server
```
systemctl status ssh
```

Open ssh port or chack that this port is open (Uncomplicated Firewall)
```
sudo ufw allow ssh
or
sudo ufw allow 4242
```

## Run SSH server
If status isn't active(running)
```
sudo systemctl start ssh
```

Autorun ssh server on system startup
```
sudo systemctl enable ssh
```

## Configure
*Client configuration* is in the file **/etc/ssh/ssh_config**

*Server configuration* is in the file  **/etc/ssh/sshd_config**
```JavaScript
Port: порт, который прослушивает процесс SSH. По умолчанию это порт 22.

PermitRootLogin: указывает, надо ли разрешить вход в систему с правами root

PermitEmptyPasswords: указывает, надо ли разрешить вход в систему с пустым паролем. По умолчанию значение - no, то есть вход с пустым паролем запрещен

X11Forwarding: указывает, надо ли разрешить клиентам использовать перенаправление X11 (графический интерфейс).
```
Update after changes
```
sudo systemctl restart ssh
```


# UFW (Uncomplicated Firewall)
UFW is a user-friendly tool for managing firewall rules on Linux systems. It simplifies the process of configuring iptables, the underlying firewall system, making it more accessible to users who find iptables syntax complex.

### Install UFW
```Bash
sudo apt install ufw
```

### Run UFW
```Bash
sudo ufw enable #run ufw
sudo ufw disable #stop ufw
sudo ufw reset  #settings to default
```

### Status
```Bash
sudo ufw status
sudo ufw status verbose
sudo ufw status numbered
```

### Configuration (default)
```Bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw delete 1 #numbered
```

### Open SSH port using ufw
```
sudo ufw allow 4242
```

# Connect/disconnection user by ssh
```
ssh <username>@<IP address> -p <PORT>
ssh <username>@127.0.0.1 -p 4242

exit
```

IP address
```Bash
ip a #inet
```

Logs
```Bash
sudo ufw logging on #low medium high full
sudo journalctl -u ssh
```

Chack connection
```
uptime
```
