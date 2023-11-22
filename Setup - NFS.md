NFS Mount required libraries
---
```Shell
sudo apt install nfs-common -y

# Add below line to /etc/fstab file after install
# truenas.local:/mnt/Truenas/Videos /mnt/Videos  nfs      defaults    0       0
```

Add below line to **"/etc/fstab"** file after install
---
```Shell
truenas.local:/mnt/Truenas/Videos /mnt/Videos  nfs      defaults    0       0
```