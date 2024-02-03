GlusterFS Package installation command
---
```Shell
sudo apt install glusterfs-server software-properties-common -y
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

Mount a volume to glusterFS cluster
---
```Shell
gluster volume create volume1 replica 3 ubt-dkr11:/mnt/data/ ubt-dkr21:/mnt/data/ ubt-dkr31:/mnt/data/
```
