
**Preliminary tests**
The defense can only occur if the student or group being evaluated is present. This way everybody learns by sharing knowledge with each other.
If no work has been submitted (or wrong files, wrong directory, or wrong filenames), the grade is 0, and the evaluation process ends.
For this project, you have to clone their Git repository on their station.
General instructions
General instructions
During the defense, if you need assistance verifying a point, the student being evaluated must assist you.
Ensure that the "signature.txt" file is present at the root of the cloned repository.
Check that the signature contained in "signature.txt" is identical to that of the ".vdi" file of the virtual machine to be evaluated. A simple "diff" should allow you to compare the two signatures. If necessary, ask the student being evaluated where their ".vdi" file is located.
As a precaution, You must duplicate the submitted virtual machine image in a different directory, then boot on that copy.
Verify that no snapshots are being used.
Do not accept a virtual machine that is already running before the defense.
Start the virtual machine to be evaluated.
If something doesn't work as expected or the two signatures differ, the evaluation stops here.
Mandatory part
The project involves creating and configuring a virtual machine according to strict rules. The student being evaluated will have to help you during the defense. Ensure that all of the following points are fulfilled.
Project overview

**The student being evaluated should explain to you simply:**
- How a virtual machine works.
	> A virtual machine (VM) works by using software called a hypervisor to create a virtualized environment that mimics a physical computer.</br>
	Host machine → The real physical computer.</br>
	Hypervisor → Special software that creates and manages virtual machines (VirtualBox).
- Their choice of operating system.
	> Debian is simple, stable, and good for many uses.
- The basic differences between Rocky and Debian.
	Rocky Linux and Debian are both Linux distributions, but they come from different “families”.</br>
	Rocky Linux → Geared toward servers, enterprise environments, and users who want RHEL compatibility without Red Hat licensing costs.</br>
	Debian → Aimed at both servers and desktops, widely used by hobbyists, developers, and as a base for many derivatives (like Ubuntu).
- The purpose of virtual machines.
	> The purpose of virtual machines is to run different systems or programs safely.
They let you test, learn, or use other operating systems without changing your main computer.
- If the evaluated student chose Rocky: what SELinux and DNF are.
- If the evaluated student chose Debian: the difference between aptitude and apt, and what APPArmor is. During the defense, a script must display information every 10 minutes. Its operation will be checked in detail later. If the explanations are not clear, the evaluation stops here.
	> **apt** and **aptitude** are both package management tools for Debian-based systems, but they differ in how they work, their interface </br>
	apt → simple command-line tool for installing and managing packages. The go-to tool for most users and scripts; simpler and faster for basic tasks.</br>
	aptitude → has more feature-rich front-end to APT with both a text-based interactive UI and command-line mode. Still valued by advanced users for its conflict resolution and interactive mode.

	> **AppArmor** is a Linux security tool.
	It controls what programs can do and what files they can access.
	This helps protect the system from attacks.</br>
	AppArmor (Application Armor) is a Linux security module that controls what programs can do, by defining a set of rules (called profiles) for each application.
	“security fence” around each program — even if the program gets hacked, it can’t step outside the fence you set for it.


**Simple setup**
Ensure that the machine does not have a graphical environment at launch. A password will be required before attempting to connect to this machine. Finally, connect with a user with the help of the student being evaluated. This user must not be root. Pay attention to the password chosen, it must follow the rules imposed in the subject.

- Check that the UFW service is started with the help of the student being evaluated.

```bash
sudo systemctl status ufw
```

- Check that the SSH service is started with the help of the student being evaluated.

```bash
sudo systemctl status ssh
```

- Check that the chosen operating system is Debian or Rocky with the help of the student being evaluated.

```bash
uname -a
```

**User**
- The subject requires that a user with the login of the student being evaluated exists
 on the virtual machine. Check that it has been added and that it belongs to the
 "sudo" and "user42" groups.

```bash
groups <username>
```

- Make sure the rules imposed in the subject concerning the password policy have been put in place by following the following steps.
First, create a new user. Assign it a password of your choice, respecting the subject rules. The student being evaluated must now explain to you how they were able to set up the rules requested in the subject on their virtual machine.

```bash
nano /etc/pam.d/common-password
useradd <username>
```

Now that you have a new user, ask the student being evaluated to create a group named "evaluating" in front of you and assign it to this user. Finally, check that this user belongs to the "evaluating" group.

```bash
groupadd evaluating
usermod -aG evaluating <username>
groups <username>
```

Finally, ask the student being evaluated to explain the advantages of this password policy, as well as the advantages and disadvantages of its implementation. Of course, answering that it is because the subject asks for it does not count.

> Advantages of password policy: It makes passwords stronger and safer. This helps protect your system from hackers.</br>
Advantages of implementing it: Better security for users and data. Less chance of password theft.</br>
Disadvantages: Users may find it hard to remember complex passwords.

**Hostname and partitions**
Check that the hostname of the machine is correctly formatted as follows: login42 (login of the student being evaluated).
Modify this hostname by replacing the login with yours, then restart the machine. If on restart, the hostname has not been updated, the evaluation stops here.

```bash
nano /etc/hostname
reboot
```

Ask the student being evaluated how to view the partitions for this virtual machine.
Compare the output with the example given in the subject. Please note: if the student being evaluated completes the bonuses, it will be necessary to refer to the bonus example.

```bash
lsblk
```

This section provides an opportunity to discuss the scores. The student being evaluated should give you a brief explanation of how LVM works and what it is all about.

> LVM (Logical Volume Manager) helps manage disk space.
It groups physical disks into one big storage pool.
You can easily resize or move parts of storage without losing data.
This makes managing disks more flexible.</br>
LVM is a flexible storage management system on Linux that lets you manage disk space more easily than traditional partitioning. Instead of fixed-size partitions, LVM lets you create “logical volumes” that can be resized, moved, or combined without much hassle.

**SUDO**
- Check that the "sudo" program is properly installed on the virtual machine.
The student being evaluated should now show assigning your new user to the "sudo" group.

```bash
sudo -v
usermod -aG sudo username
```

- The subject mandates strict rules for sudo. The student being evaluated must first explain the value and operation of sudo using examples of their choice. In a second step, it must show you the implementation of the rules imposed by the subject.

> **Sudo** stands for “superuser do”. It’s a command in Unix/Linux systems that allows a regular user to run commands with superuser (root) privileges temporarily. The superuser (root) has unrestricted access to the system — installing software, modifying system files, changing configurations, managing users, etc.

> Why use sudo?</br>
Security: Instead of logging in as root (which is risky), users get temporary elevated rights only when needed.</br>
Accountability: Actions done via sudo are logged, so you can track who did what.</br>
Safety: Limits the time and scope of admin privileges — mistakes or malicious actions are less likely.

```bash
sudo visudo
```

- Verify that the "/var/log/sudo/" folder exists and has at least one file. Check the contents of the files in this folder, You should see a history of the commands used with sudo.

```bash
cd /var/log/sudo
sudo tail /var/log/sudo/sudo.log
```

- inally, try to run a command via sudo. See if the file (s) in the "/var/log/sudo/" folder have been updated.

```bash
sudo ls
sudo tail /var/log/sudo/sudo.log
```

**UFW / Firewalld**
- Check that the "UFW" (or "Firewalld" for rocky) program is properly installed on the virtual machine. Check that it is working properly.

```bash
sudo ufw status
sudo systemctl status ufw
```

- The student being evaluated should provide a basic explanation of what UFW (or Firewalld) is and its value.

> UFW (Uncomplicated Firewall) is firewall management tool for Linux. UFW acts as a front-end to the more complex iptables firewall system, making it easier to configure basic firewall rules.</br>
Controls incoming and outgoing network traffic by allowing or denying connections based on rules you set.
Lets you open or close ports (like SSH on port 22, HTTP on port 80) with simple commands.
Helps protect your system from unauthorized access and network attacks.

- List the active rules in UFW (or Firewall). A rule must exist for port 4242.

```bash
sudo ufw status numbered
```

- Add a new rule to open port 8080. Check that this one has been added by listing the active rules. Finally, delete this new rule with the help of the student being evaluated.

```bash
sudo ufw allow 8080
sudo ufw status
sudo ufw delete allow 8080
sudo ufw status numbered
sudo ufw delete 2
```

**SSH**
Check that the SSH service is properly installed on the virtual machine.
Check that it is working properly.

```bash
sudo systemctl status ssh
```

The student being evaluated must be able to explain to you basically what SSH is and the value of using it.
Verify that the SSH service only uses port 4242 in the virtual machine.
The student being evaluated should help you use SSH in order to log in with the newly created user. To do this, you can use a key or a simple password. It will depend on the student being evaluated. Of course, you have to make sure that you cannot use SSH with the "root" user as stated in the subject. If something does not work as expected or is not clearly explained, the evaluation stops here.

**Script monitoring**
The student being evaluated should explain to you simply:
- How their script works by showing you the code.
```bash
cat /usr/local/bin/monitoring.sh
```

- What "cron" is.
> Cron is a tool in Linux that runs commands automatically at set times or dates.</br>
It is used for tasks like backups, updates, or sending reports.

How the student being evaluated set up their script so that it runs every 10 minutes from when the server starts.

```bash
crontab -e
```

Once the script's correct functioning has been verified, the student being evaluated should ensure that it runs every minute. You can run whatever you want to make sure the script runs with dynamic values correctly.

Finally, the student being evaluated should make the script stop running when the server has started up, but without modifying the script itself. To check this point, you will have to restart the server one last time. At startup, it will be necessary to check that the script still exists in the same place, that its rights have remained unchanged, and that it has not been modified.
**Bonus**
Check, with the help of the subject and the student being evaluated, the bonus
 points authorized for this project:
Setting up partitions is worth 2 points.
Setting up WordPress, only with the services required by the subject, is worth 2 points.
The free choice service is worth 1 point. Verify and test the proper functioning and implementation of each extra service. For the free choice service, the student being evaluated must provide a simple explanation of how it works and why they think it is useful. Please note that NGINX and Apache2 are prohibited.
