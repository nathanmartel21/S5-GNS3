### Microcore TA1 :

---------------

```bash
tce-load –wi tc-install
reboot

sudo vi /opt/bootsync.sh
/usr/bin/sethostname TA1

tce-load -wi iproute2
tce-load -wi iptables
sudo modprobe ipv6

touch /opt/eth0.sh && chmod 755 /opt/eth0.sh
#!/bin/sh
pkill udhcpc
sudo ip addr add 200.8.99.1/24 dev eth0
sudo ip route add default via 200.8.99.254 dev eth0
sudo ip -6 addr add 2001:0:8:99::1/64 dev eth0
sudo ip -6 route add default via 2001:0:8:99::254
sudo echo "nameserver 8.8.8.8" > /etc/resolv.conf
echo "/opt/eth0.sh" >> /opt/bootlocal.sh

echo "nameserver 159.31.11.5" >> /etc/resolv.conf #prendre le DNS de l'école (en ayant le NAT fait avant)
echo "nameserver 159.31.10.4" >> /etc/resolv.conf #prendre le DNS de l'école (en ayant le NAT fait avant)
echo "nameserver 159.31.10.2" >> /etc/resolv.conf #prendre le DNS de l'école (en ayant le NAT fait avant)
echo "nameserver 159.31.20.27" >> /etc/resolv.conf #prendre le DNS de l'école (en ayant le NAT fait avant)

tce-load -wi dropbear
dbclient vyos@200.8.99.254

filetool.sh -b
```
