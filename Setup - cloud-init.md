Update packages
---
```Shell
apt-get update && apt-get upgrade -y && apt dist-upgrade -y
```

Installing Dependencies
---
```Shell
apt install -y cloud-initramfs-growroot qemu-guest-agent cifs-utils bridge-utils avahi-daemon python3-pip net-tools cloud-init
```

Disable Password authentication
---
```Shell
sed -i 's/^#*\s*PasswordAuthentication.*/PasswordAuthentication no/' /etc/ssh/sshd_config
```

Enable user auto sudo for user
---
```Shell
touch /etc/sudoers.d/010_${new_username}-nopasswd
echo "${new_username} ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/010_${new_username}-nopasswd
```

Delete the default username
---
```Shell
deluser --remove-home ubuntu
```

Remove the host keys
---
``` Shell
rm -rf /etc/ssh/ssh_host_*
```

Remove the machine-id
---
```Shell
truncate -s 0 /etc/machine-id
ls -l /var/lib/dbus/machine-id
ls -s /etc/machine-id /var/lib/dbus/machine-id
```

Clean the packages
---
```Shell
apt clean
apt autoremove
```