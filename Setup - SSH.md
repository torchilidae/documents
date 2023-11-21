Creation of ssh keys
---
```Shell
ssh-keygen -t rsa -b 4096
```
Updating ssh keys in remote host
---
```Shell
ssh ${user}@${hostname} mkdir -p .ssh
cat .ssh/id_rsa.pub | ssh ${user}@${hostname} 'cat >> .ssh/authorized_keys'
ssh ${user}@${hostname} "chmod 700 .ssh; chmod 640 .ssh/authorized_keys"
```
Disable password authentication on the servers
---
```Shell
sed -i 's/^#*\s*PasswordAuthentication.*/PasswordAuthentication no/' /etc/ssh/sshd_config
```