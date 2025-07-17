**LVM - logical valume manager**

# Creating a new partition
First, add a new disk to the VM

## 1. Encrypt the partitions with cryptsetup

```
sudo cryptsetup luksFormat /dev/sdb
```
YES, PASSPHRASE

## 2. Open them (this will create /dev/mapper/secure1)
Manual mounting
```
sudo cryptsetup open /dev/sdb secure1
```
Now you see /dev/mapper/secure1 — it behaves like a regular block device.

## 3. Create Physical Volumes (PV) from them
```
sudo pvcreate /dev/mapper/secure1
```

## 4. Create one Volume Group (VG), for example vgcrypt
```
sudo vgcreate vgcrypt /dev/mapper/secure1
```

## 5. Create Logical Volumes (LV) inside the VG
```
sudo lvcreate -l 50%VG -n lv_data1 vgcrypt
sudo lvcreate -l 100%FREE -n lv_data2 vgcrypt
```



## 6. Format volumes and mount them
Create the volumes
```
sudo mkfs.ext4 /dev/vgcrypt/lv_data1
sudo mkfs.ext4 /dev/vgcrypt/lv_data2
```

Create mount points (folders)
```
sudo mkdir /mnt/secure1
sudo mkdir /mnt/secure2'
```

Mount the volumes (link volumes to folder)
```
sudo mount /dev/vgcrypt/lv_data1 /mnt/secure1
sudo mount /dev/vgcrypt/lv_data2 /mnt/secure2
```


# Automatically mount at startup
## 1. Add to /etc/crypttab
```
#secure1 UUID=<UUID-sdb1> none luks
secure1 dev/sdb none luks
```
To get the UUID:

```
sudo blkid /dev/sdb
```
## 2. Add to /etc/fstab
```
#UUID=<UUID_lv_data1> /mnt/secure1 ext4 defaults 0 2
#UUID=<UUID_lv_data2> /mnt/secure2 ext4 defaults 0 2

/dev/mapper/vgcrypt-lv_data1  /mnt/secure1 ext4 defaults 0 2
/dev/mapper/vgcrypt-lv_data2  /mnt/secure2 ext4 defaults 0 2
```
To get the UUID of the logical volume:
```
sudo blkid /dev/vgcrypt/lv_data1
sudo blkid /dev/vgcrypt/lv_data2
```
## 3. Verify:
```
sudo update-initramfs -u
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl restart systemd-cryptsetup@secure1.service
```