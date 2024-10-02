Installation of the required packages in Proxmox
---
```Shell
sudo apt update -y && sudo apt install libguestfs-tools -y
```
Download the image files to the Proxmox
---
```Shell
wget https://cloud.debian.org/images/cloud/bookworm/latest/debian-12-generic-amd64.qcow2
```
Check for password authentication on the image file and disable it using the below commands
---
```Shell
# To check for the password
virt-cat -a debian-12-generic-amd64.qcow2 /etc/ssh/sshd_config | grep PasswordAuthentication
# To update the password
virt-edit -a debian-12-generic-amd64.qcow2 /etc/ssh/sshd_config -e 's/^#*\s*PasswordAuthentication.*/PasswordAuthentication no/'
```
Download the required packages to the image file
---
```Shell
virt-customize -a debian-12-generic-amd64.qcow2 --install qemu-guest-agent,cloud-initramfs-growroot,cifs-utils,bridge-utils,avahi-daemon,python3-pip,net-tools,cloud-init
```
Cleanup "/etc/machine-id" created after the required packages installation
---
```Shell
virt-customize -a debian-12-generic-amd64.qcow2 --delete /etc/machine-id
```
Convert the file from qcow2 to raw format for it to be compatable with Ceph
---
```Shell
qemu-img convert -f qcow2 -O raw debian-12-generic-amd64.qcow2 debian.raw
```
Creating a vm template from the created disk
---
```Shell
qm create 401 --name "deb-template" --memory 2048 --cores 2 --net0 virtio,bridge=vmbr0
qm importdisk 401 debian.raw local-lvm
qm set 401 --scsihw virtio-scsi-pci --scsi0 local-lvm:vm-401-disk-0 --ide2 local-lvm:cloudinit
qm set 401 --boot c --bootdisk scsi0 --agent enabled=1

qm template 401
```


For Ubuntu Image below are the commands
---
```Shell
cd /mnt/pve/ds/template/iso/
wget https://cloud-images.ubuntu.com/jammy/current/jammy-server-cloudimg-amd64.img
virt-edit -a jammy-server-cloudimg-amd64.img /etc/ssh/sshd_config -e 's/^#*\s*PasswordAuthentication\ yes/PasswordAuthentication no/'
virt-cat -a jammy-server-cloudimg-amd64.img /etc/ssh/sshd_config | grep PasswordAuthentication
virt-customize -a jammy-server-cloudimg-amd64.img --install qemu-guest-agent,avahi-daemon -x
# virt-customize -a jammy-server-cloudimg-amd64.img --install qemu-guest-agent,cloud-initramfs-growroot,cifs-utils,bridge-utils,avahi-daemon,cloud-init -x
# virt-edit -a jammy-server-cloudimg-amd64.img /etc/machine-id -e 's/.*//'
guestfish -a jammy-server-cloudimg-amd64.img --rw
><fs> run
><fs> mount /dev/sda1 /
><fs> edit /etc/cloud/cloud.cfg.d/91_packages.cfg

# This portion contains the package list of the required software on the servers
package_update: true
package_upgrade: true

packages:
  - qemu-guest-agent
  - avahi-daemon

# Restart after install of the packages
power_state:
  delay: 2 
  mode: reboot
  message: "Coludinit completed execution. Rebooting"
  condition: true
  
qemu-img convert -f qcow2 -O raw jammy-server-cloudimg-amd64.img ubuntu.raw
```

Creating a vm template for ubuntu
---
```Shell
qm destroy 401
qm create 401 --name "ubt-template" --memory 2048 --cores 2 --net0 virtio,bridge=vmbr0
qm importdisk 401 ubuntu.raw local-lvm
qm set 401 --scsihw virtio-scsi-pci --scsi0 local-lvm:vm-401-disk-0 --ide2 local-lvm:cloudinit
qm set 401 --boot c --bootdisk scsi0 --agent enabled=1
qm template 401

qm destroy 402
qm create 402 --name "ubt-template" --memory 2048 --cores 2 --net0 virtio,bridge=vmbr0
qm importdisk 402 ubuntu.raw local-lvm
qm set 402 --scsihw virtio-scsi-pci --scsi0 local-lvm:vm-402-disk-0 --ide2 local-lvm:cloudinit
qm set 402 --boot c --bootdisk scsi0 --agent enabled=1
qm template 402

qm destroy 403
qm create 403 --name "ubt-template" --memory 2048 --cores 2 --net0 virtio,bridge=vmbr0
qm importdisk 403 ubuntu.raw local-lvm
qm set 403 --scsihw virtio-scsi-pci --scsi0 local-lvm:vm-403-disk-0 --ide2 local-lvm:cloudinit
qm set 403 --boot c --bootdisk scsi0 --agent enabled=1
qm template 403
```