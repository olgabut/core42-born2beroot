# APT (Advanced Package Tool)
**APT** is the primary command-line package manager.
It provides tools for managing software packages, including installing, removing, updating, and querying information about them.

# Aptitude
**Aptitude** is a package manager for Debian-based systems. It is a front-end to dpkg and apt, offering both:
- a command-line interface (CLI), and
- a text-based user interface (TUI)

## Comparison with apt and aptitude
| Feature               | `apt`     | `aptitude`              |
| --------------------- | --------- | ----------------------- |
| Default installed     | Yes       | No (install manually)   |
| Dependency resolution | Automatic | Offers multiple options |
| Interface             | CLI only  | CLI + Text UI           |


## Example APT Usage:
### Update package lists:
```
sudo apt update
```

### Upgrade installed packages:
```
sudo apt upgrade
```

### Install a package:
```
sudo apt install <package_name>
```

### Remove a package:
```
sudo apt remove <package_name>
```

### Remove a package and its configuration files:
```
sudo apt purge <package_name>
```
### Search for a package:
```
apt search <keyword>
```