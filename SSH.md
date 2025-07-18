# SSH (Secure Shell)


## Install OpenSSH Server
```
sudo apt update
sudo apt install openssh-server
```

## Status SSH server
```
systemctl status ssh
```

Chack that the PORT is open
```
sudo ufw allow ssh
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

PermitRootLogin: указывает, надо ли вы разрешить вход в систему с правами root

PermitEmptyPasswords: указывает, надо ли вы разрешить вход в систему с пустым паролем. По умолчанию значение - no, то есть вход с пустым паролем запрещен

X11Forwarding: указывает, надо ли вы разрешить клиентам использовать перенаправление X11.
```
Update after changes
```
sudo systemctl restart ssh
```


# UFW (Uncomplicated Firewall)
UFW is a user-friendly tool for managing firewall rules on Linux systems. It simplifies the process of configuring iptables, the underlying firewall system, making it more accessible to users who find iptables syntax complex.

### Install UFW
```
sudo apt install ufw
```

### Run UFW
```Java
sudo ufw enable //run ufw
sudo ufw disable //stop ufw
sudo ufw reset // settings to default
```

### Status
```
sudo ufw status
sudo ufw status verbose
sudo ufw status numbered
```

### Configuration (default)
```
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw delete 1 //numbered
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
```JavaScript
ip a //inet
```

Logs
```Java
sudo ufw logging on //low medium high full
sudo journalctl -u ssh
```

Chack connection
```
uptime
```

