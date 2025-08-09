# cron
**The cron command-line utility** is a job scheduler on Unix-like operating systems.
Users who set up and maintain software environments use cron to schedule jobs (commands or shell scripts),
also known as cron jobs, to run periodically at fixed times, dates, or intervals.

System ctontab is in the file **/etc/crontab**

```bash
# добавляем скрипт в расписание
$crontab -e
*/10 * * * *	sh /home/<user>/monitoring.sh

  * * * * * <command to execute>
# | | | | |
# | | | | day of the week (0–6) (Sunday to Saturday;
# | | | month (1–12)             7 is also Sunday on some systems)
# | | day of the month (1–31)
# | hour (0–23)
# minute (0–59)

```


Create script file
**/usr/local/bin/monitoring.sh**
```bash
sudo touch /usr/local/bin/monitoring.sh
sudo chmod 755 /usr/local/bin/monitoring.sh
```

Run script without password
```bash
sudo visudo
```
Add in file
```bash
USENAME  ALL=(ALL) NOPASSWD: /usr/local/bin/monitoring.sh
%sudo ALL=(ALL:ALL) NOPASSWD: /usr/local/bin/monitoring.sh
```

sudo systemctl enable cron.service

```bash
#!/bin/bash
# 1. System architecture and kernel version
ARCH=$(uname -a)

# 2. lscpu provides CPU info. "Socket(s)" tells how many physical CPUs are installed
PHYSICAL_CPUS=$(lscpu | grep "Socket(s):" | awk '{print $2}')

# 3. CPU(s)" shows total logical processors (including hyper-threading)
VIRTUAL_CPUS=$(lscpu | grep "^CPU(s):" | awk '{print $2}')

# 4. free -m shows memory usage in MB
RAM_TOTAL=$(free -m | awk '/Mem:/ {print $2}')
RAM_USED=$(free -m | awk '/Mem:/ {print $3}')
RAM_PERCENT=$(( RAM_USED * 100 / RAM_TOTAL ))

# 5. df -BG gives a total view of all mounted disks in GB
DISK_TOTAL=$(df -BG | grep '^/dev/' | grep -v 'boot$' | awk '{disk_t += $2} END {print disk_t}')
DISK_USED=$(df -BM | grep '^/dev/' | grep -v 'boot$' | awk '{disk_u += $3} END {print disk_u}')
DISK_PERCENT=$(( DISK_USED * 100 / (DISK_TOTAL * 1024) ))

# 6. top -bn1 runs top non-interactively. The idle percentage is $8, so subtract from 100.
CPU_LOAD=$(mpstat 1 1 | awk '/^Average:/ || /^[0-9]/ {print 100 - $12}' | tail -n 1)

# 7. Shows last system boot time.
LAST_BOOT=$(who -b | awk '{print $3 " " $4}')

# 8. If lsblk shows "lvm", then LVM is used.
LVM_USE=$(lsblk | grep -q "lvm" && echo yes || echo no)

# 9. Count how many established TCP connections exist.
TCP_CONN=$(ss -Ht state established | wc -l)

# 10. who lists logged-in users; count lines.
USER_LOG=$(who | wc -l)

# 11. Get the first IPv4 address and MAC address.
IP=$(hostname -I | awk '{print $1}')
MAC=$(ip link show | awk '/ether/ {print $2}' | head -n 1)

# 12. Count how many times sudo has been used
SUDO_CMD=$(journalctl _COMM=sudo 2>/dev/null | grep COMMAND | wc -l)

MESSAGE=$(cat <<EOF
#Architecture: $ARCH
#CPU physical : $PHYSICAL_CPUS
#vCPU : $VIRTUAL_CPUS
#Memory Usage: $RAM_USED/${RAM_TOTAL}MB (${RAM_PERCENT}%)
#Disk Usage: $DISK_USED/${DISK_TOTAL}Gb ({$DISK_PERCENT}%)
#CPU load: ${CPU_LOAD}%
#Last boot: $LAST_BOOT
#LVM use: $LVM_USE
#Connections TCP: ${TCP_CONN} ESTABLISHED
#User log: $USER_LOG
#Network: IP $IP ($MAC)
#Sudo : ${SUDO_CMD} cmd
EOF
)

wall "$MESSAGE"
```


⏱️ Make it run every 10 minutes (with cron)
Make the script executable:

bash
Copy
Edit
chmod +x monitoring.sh
Edit crontab:

bash
Copy
Edit
sudo crontab -e
Add this line:

bash
Copy
Edit
*/10 * * * * /path/to/monitoring.sh
