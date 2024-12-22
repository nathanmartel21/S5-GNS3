### Microcore TD2 :

---------------

```bash
tce-load –wi tc-install
reboot

sudo vi /opt/bootsync.sh
/usr/bin/sethostname TD2

tce-load -wi iproute2
tce-load -wi iptables
sudo modprobe ipv6

touch /opt/eth0.sh && chmod 755 /opt/eth0.sh
#!/bin/sh
udhcpc -i eth0

echo "/opt/eth0.sh" >> /opt/bootlocal.sh

filetool.sh -b
```

