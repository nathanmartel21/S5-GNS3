### OpenVSwitch1 :

---------------

```bash
ovs-vsctl set port eth1 tag=10
ovs-vsctl set port eth2 tag=10
ovs-vsctl set port eth3 trunks=10,20,99
ovs-vsctl set port eth4 tag=99
ovs-vsctl set port eth5 tag=20
ovs-vsctl set port eth6 tag=20

# Pour pouvoir ping le switch : 

ovs-vsctl add-port br0 vlan10 tag=10 -- set interface vlan10 type=internal
ip addr add 200.8.10.123/24 dev vlan10
ip link set vlan10 up

ovs-vsctl add-port br0 vlan20 tag=20 -- set interface vlan20 type=internal
ip addr add 200.8.20.123/24 dev vlan20
ip link set vlan20 up

ovs-vsctl add-port br0 vlan99 tag=99 -- set interface vlan99 type=internal
ip addr add 200.8.99.123/24 dev vlan99
ip link set vlan99 up

ovs-vsctl show
ovs-vsctl remove port eth0 tag 10
ovs-vsctl del-port vlan10
```
