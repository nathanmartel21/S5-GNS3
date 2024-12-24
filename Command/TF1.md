### Microcore TF1 :

---------------

```bash
tce-load –wi tc-install
reboot

sudo vi /opt/bootsync.sh
/usr/bin/sethostname TF1

tce-load -wi iproute2
tce-load -wi iptables
sudo modprobe ipv6

echo "sudo modprobe ipv6" >> /opt/bootsync.sh

touch /opt/eth0.sh && chmod 755 /opt/eth0.sh
#!/bin/sh
pkill udhcpc
sudo ip addr add 200.8.20.1/24 dev eth0
sudo ip route add default via 200.8.20.254 dev eth0
sudo ip -6 addr add 2001:0:8:20::1/64 dev eth0
sudo ip -6 route add default via 2001:0:8:20::254
sudo echo "nameserver 8.8.8.8" > /etc/resolv.conf
echo "/opt/eth0.sh" >> /opt/bootlocal.sh

filetool.sh -b
```

