## If Jenkins Build in node is not running due to /temp memory
**Use these steps to resolve**
**Run**
```bash
sudo mount -o remount,size=2G /tmp
```
**Verify**
```bash
df -h /tmp
```
**You should now see 2.0Gb**


**Make Permanent After Reboot**
**Edit fstab**
```bash
sudo vi /etc/fstab
```
**Add this line at the bottom, save, exit & restart. By now the error will be gone and the build it node will come up after restart**
```bash
/tmp tmpfs defaults,size=2G 0 0
```
