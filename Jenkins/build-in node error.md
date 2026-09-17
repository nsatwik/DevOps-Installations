# Check current /tmp size
```
df -hT /tmp
```
# Create systemd override directory
```sudo mkdir -p /etc/systemd/system/tmp.mount.d```

# Edit /tmp mount configuration
sudo vi /etc/systemd/system/tmp.mount.d/override.conf

Add
[Mount]
Options=mode=1777,strictatime,nosuid,nodev,size=2G,nr_inodes=1m,x-systemd.graceful-option=usrquota
