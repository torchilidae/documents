GlusterFS Package installation command
---
```Shell
sudo apt install glusterfs-server software-properties-common certmonger -y
```

Enable GlusterFS service
---
```Shell
sudo systemctl enable glusterd.service
sudo systemctl start glusterd.service
sudo systemctl status glusterd.service
```

Node status and addition of new nodes to the cluster
---
```Shell
# Check the status and add node to the cluster
sudo gluster peer status
sudo gluster peer probe ${nodename}
# Ex: sudo gluster peer probe master
```

Creation and resubmission of SSL cert for encryption
---
```Shell
getcert request -r -K host/$(hostname) -f /etc/ssl/gluster.pem -k /etc/ssl/gluster.key -D $(hostname) -F /etc/ssl/gluster.ca
getcert resubmit -r -K host/$(hostname -f) -f /etc/ssl/gluster.pem -k /etc/ssl/gluster.key -D $(hostname -f) -F /etc/ssl/gluster.ca
```

Prepare the GlusterFS cluster for data storage
---
```Shell
mkdir /mnt/data/brick1
```


Mount a volume to glusterFS cluster
---
```Shell
gluster volume create volume1 replica 3 ubt-dkr11:/mnt/data/brick1 ubt-dkr21:/mnt/data/brick1 ubt-dkr31:/mnt/data/brick1
```

Start the volume
---
```Shell
gluster volume start volume1
```

Check the status of the volume
--- 
```Shell
gluster volume status
```

Mount the volume to fstab
--- 
```Shell
echo "$(hostname).tsphotoclicks.net:/volume_internal /mnt/volume_internal  glusterfs defaults,_netdev 0 0" | sudo tee -a /etc/fstab
```