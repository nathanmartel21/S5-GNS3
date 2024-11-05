# Exercice S5-GNS3 (12, 24)

## 1 Liste des contraintes

### 1.1 Les serveurs

**Contraintes 1 (20 pts) :** Les serveurs TS1, TS2 et TS99 sont des machines virtuelles MicroCore. Chaque serveur dispose de 1 interface Ethernet. Les serveurs sont nommés.
- Serveurs TS1, TS2 et TS99 OK. Redirection du stdout vers ttySO OK. Les serveurs sont nommés en modifiant dans le fichier /opt/bootsync.sh : /usr/bin/sethostname = TS[n].
- Date : `Fait` le 02/11/2024

**Contraintes 2 (5pts) :** Le serveur TS2 est connecté au réseau IPv4 N02. Le serveur TS1 est connecté au réseau IPv4 N01.
- Adressage persistent OK.
```bash
tce-load -wi iproute2
tce-load -wi iptables
sudo modprobe ipv6
touch /opt/eth0.sh

#!/bin/sh
sudo ip addr add 200.8.[1|2].1/24 dev eth0
sudo ip route add default via 200.8.[1|2].254 dev eth0
sudo ip -6 addr add 2001:0:8:[1|2]::1/64 dev eth0
sudo ip -6 route add default via 2001:0:8:1::254
```
- Mettre dans /opt/bootsync.sh **et avant le l'activation du getty !!** : sudo modprobe ipv6 et /opt/eth0.sh
- Sauvegarder les modifications :
```bash
  filetool.sh -b
```
- Date : `Fait` le 03/11/2024

**Contraintes 3 (1pt) :** Le serveur TS99 est connecté aux réseaux IPv4 N10, N20 et N99.
- Dans le fichier /opt/eth0.sh (contrainte 2 + ..) :
```bash
vconfig add eth0 10
vconfig add eth0 20
vconfig add eth0 99
ip link set dev eth0 up
ip link set dev eth0.10 up
ip link set dev eth0.20 up
ip link set dev eth0.99 up

ip addr add 200.8.10.100/24 dev eth0.10
ip -6 addr add 2001:0:8:10::100/64 dev eth0.10
ip addr add 200.8.20.100/24 dev eth0.20
ip -6 addr add 2001:0:8:20::100/64 dev eth0.20
ip addr add 200.8.99.100/24 dev eth0.99
ip -6 addr add 2001:0:8:99::100/64 dev eth0.99

ip route add default via 200.8.99.254 dev eth0.99
ip -6 route add default via 2001:0:8:99::254 dev eth0.99

echo "nameserver 8.8.8.8" > /etc/resolv.conf
```
- Date : `Fait` le 04/11/2024

**Contraintes 4 (1pt) :** Le serveur TS2 est connecté au réseau IPv6 N02. Le serveur TS1 est connecté au réseau IPv6 N01.
- Cf contrainte 2
- Date : `Fait` le 04/11/2024


**Contraintes 5 (2pts) :** Les adresses IPv4 et IPv6 des interfaces réseaux des serveurs sont statiques.
- Cf contrainte 2
- Date : `Fait` le 04/11/2024

**Contraintes 6 (2pts) :** Le serveur TS99 fournit le service DHCP (IPv4) pour les réseaux N10 et N20.
- Créer deux fichier udhcpd-[10|20].conf
```bash
start 200.8.[10|20].10
end 200.8.[10|20].250
interface eth0.10 #interface qui "donne" les @IPs, celle de sortie
option subnet 255.255.255.0
option router 200.8.10.254 #@ip du routeur
```
- Et dans /opt/bootsync.sh (après l'adressage des interfaces, après le script eth0.sh :
```bash
udhcpd /opt/udhcpd-10.conf
udhcpd /opt/udhcpd-20.conf
```
- Date : `Fait` le 04/11/2024

### 1.2 Les terminaux

**Contraintes 7 (20pts) :** Les terminaux TD1, TD2, TF1, TF2 et TA1 sont des machines virtuelles MicroCore. Chaque serveur dispose de 1 interface Ethernet. Les terminaux sont nommés.
- Cf contrainte 1
- Date : `Fait` le 03/11/2024

**Contraintes 8 (5pts) :** Les terminaux TD1 et TD2 sont connectés au réseau IPv4 N10. Les terminaux TF1 et TF2 sont connectés au réseau IPv4 N20. Le terminal TA1 est connectés
au réseau IPv4 N99.
- Cf contrainte 2
- Date : `Fait` le 04/11/2024

**Contraintes 9 (1pts) :** Les terminaux TD1 et TD2 sont connectés au réseau IPv6 N10. Les terminaux TF1 et TF2 sont connectés au réseau IPv6 N20. Le terminal TA1 est connectés
au réseau IPv6 N99.
- Cf contrainte 2
- Date : `Fait` le 04/11/2024

**Contraintes 10 (2pts) :** Les adresses IPv4 des interfaces réseaux des terminaux TD1, TF1 et TA1 sont statiques.
- Cf contrainte 2
- Date : `Fait` le 04/11/2024

**Contraintes 11 (2pts) :** Les adresses IPv4 des interfaces réseaux des terminaux TD2 et TF2 sont dynamiques (DHCP).
- Dans le fichier /opt/eth0.sh :
```bash
sudo udhcpc -i eth0
```
- Ne pas oublier de mettre quand même une route par défaut (#ip route add default via)
- Date : `Fait` le 04/11/2024

**Contraintes 12 (1pt) :** Les adresses IPv6 des interfaces réseaux des terminaux TD2 et TF2 sont dynamiques (autoconfiguration IPv6).
- Non fait
- Date :

**Contraintes 13 (2pts) :** Le terminal TD1 peut « pinguer » en IPv4 le terminal TF1. 
- Dans le Open VSwitch, mettre en mode accès les endpoints (et en trunk les endpoints avec n sous interfaces)
```bash
ovs-vsctl set port eth1 tag=10
ovs-vsctl set port eth2 tag=10
ovs-vsctl set port eth3 trunks=10,20,99
ovs-vsctl set port eth4 tag=99
ovs-vsctl set port eth5 tag=20
ovs-vsctl set port eth6 tag=20
```
- Date : `Fait` le 04/11/2024

**Contraintes 14 (2pts) :** Le terminal TD1 peut « pinguer » en IPv4 le terminal TD2.
- Cf contrainte 15
- Date : `Fait` le 04/11/2024

**Contraintes 15 (2pts) :** Le terminal TD1 peut « pinguer » en IPv4 le serveur TS2.
- Cf contrainte 15
- Date : `Fait` le 04/11/2024

**Contraintes 16 (1pt) :** Le terminal TD1 peut « pinguer » l’adresse 8.8.8.8.
- Sur R2 :
  ```bash
  vyos@R2# set nat source rule 100 outbound-interface name eth1 #interface de sortie (outside)
  vyos@R2# set nat source rule 100 source address 200.8.1.0/24 #plage translatée
  vyos@R2# set nat source rule 100 translation address masquerade #overload

  vyos@R2# set nat source rule 101 outbound-interface name eth1 #interface de sortie (outside)
  vyos@R2# set nat source rule 101 source address 200.8.2.0/24 #plage translatée
  vyos@R2# set nat source rule 101 translation address masquerade #overload

  vyos@R2# set nat source rule 102 outbound-interface name eth1 #interface de sortie (outside)
  vyos@R2# set nat source rule 102 source address 200.8.10.0/24 #plage translatée
  vyos@R2# set nat source rule 102 translation address masquerade #overload

  vyos@R2# set nat source rule 103 outbound-interface name eth1 #interface de sortie (outside)
  vyos@R2# set nat source rule 103 source address 200.8.20.0/24 #plage translatée
  vyos@R2# set nat source rule 103 translation address masquerade #overload

  vyos@R2# set nat source rule 104 outbound-interface name eth1 #interface de sortie (outside)
  vyos@R2# set nat source rule 104 source address 200.8.99.0/24 #plage translatée
  vyos@R2# set nat source rule 104 translation address masquerade #overload
  
  commit
  save
  exit
  ```
- Date : `Fait` le 03/11/2024

**Contraintes 17 (1pt) :** Le terminal TD1 peut « pinguer » en IPv6 le terminal TF1.
- Cf contrainte 15
- Date : `Fait` le 04/11/2024

**Contraintes 18 (1pt) :** Le terminal TD1 peut « pinguer » en IPv6 le terminal TD2.
- Cf contrainte 15
- Date : `Fait` le 04/11/2024

**Contraintes 19 (1pt) :** Le terminal TD1 peut « pinguer » en IPv6 le serveur TS2.
- Cf contrainte 2
- Date : `Fait` le 05/11/2024
- 
### 1.3 Le routeur R1

**Contraintes 20 (20pts) :** Le routeur R1 est une machine virtuelle VyOS. Il dispose de 2 interfaces Ethernet. Le routeur R1 est nommé.
- Dans le routeur Vyos :
  ```bash
  configure
  set system host-name R1
  commit
  save
  exit
  ```
- Date : `Fait` le 03/11/2024

**Contraintes 21 (5pts) :** Le routeur R1 est connecté au réseau IPv4 N01, N10, N20 et N99.
- Dans VyOS OSPFv2 :
  ```bash
  configure
  set interfaces loopback lo address 1.1.1.1/24
  set protocols ospf parameters router-id 1.1.1.1
  set protocols ospf area 0 network 200.8.1.0/24
  set protocols ospf area 0 network 200.8.10.0/24
  set protocols ospf area 0 network 200.8.20.0/24
  set protocols ospf area 0 network 200.8.99.0/24
  ``` 
  <!--set protocols ospf default-information originate always
  set protocols ospf default-information originate metric 10
  set protocols ospf default-information originate metric-type 2-->
  ``` 
- Date : `Fait` le 04/11/2024

**Contraintes 22 (1pt) :** Le routeur R1 est connecté au réseau IPv6 N01, N10, N20 et N99.
- Fait avec les commandes OSPFv3 suivante :
```bash
configure
set protocols ospfv3 area 0
set protocols ospfv3 paraleters router-id 1.1.1.1
--
set protocols ospfv3 interface eth0 area 0 #interface directement connecté à l'autre routeur
set protocols ospfv3 redistribute connected
```
- Date : `Fait` le 04/11/2024

**Contraintes 23 (2pts) :** Le routeur R1 utilise le protocole OSPF pour la construction de sa table de routage.
- Cf contrainte 21
- Date : `Fait` le 04/11/2024

**Contraintes 24 (2pts) :** Le routeur R1 est accessible en SSH depuis le terminal TA1.
- Non fait
- Date :

### 1.4 Le routeur R2

**Contraintes 25 (20pts) :** Le routeur R2 est une machine virtuelle VyOS. Il dispose de 3 interfaces Ethernet. Le routeur R2 est nommé.
- 
- Date : `Fait` le 04/11/2024

**Contraintes 26 (5pts) :** Le routeur R2 est connecté au réseau IPv4 N01 et N02.
- Adressage :
  ```bash
  configure
  set interfaces ethernet eth[n] address W.X.Y.Z/24
  commit
  save
  ``` 
- Date : `Fait` le 04/11/2024

**Contraintes 27 (1pt) :** Le routeur R2 est connecté au réseau IPv6 N01 et N02.
- Adressage :
  ```bash
  configure
  set interfaces ethernet eth[n] address W:X:Y::Z/64
  commit
  save
  ``` 
- Date : `Fait` le 04/11/2024

**Contraintes 28 (10pts) :** Le routeur R2 est connecté à un noeud NAT ou Cloud de GNS3.
- Cablage. Et request en DHCP
- Date : `Fait` le 04/11/2024

**Contraintes 29 (2pts) :** Le routeur R2 utilise le protocole OSPF pour la construction de sa table de routage IPv4.
- Dans VyOS OSPFv2 :
  ```bash
  configure
  set interfaces loopback lo address 2.2.2.2/24
  set protocols ospf parameters router-id 2.2.2.2
  set protocols ospf area 0 network 200.8.1.0/24
  set protocols ospf area 0 network 200.8.2.0/24
  ```
  <!--
  set protocols ospf default-information originate always
  set protocols ospf default-information originate metric 10
  set protocols ospf default-information originate metric-type 2-->
- Date : `Fait` le 04/11/2024

**Contraintes 30 (1pt) :** Le routeur R2 utilise le protocole OSPF pour la construction de sa table de routage IPv6.
- Commande OSPFv3 :
  ```bash
  configure
  set protocols ospfv3 area 0
  set protocols ospfv3 parameters router-id 2.2.2.2
  --
  set protocols ospfv3 interface eth2 area 0 #interface directement connecté à l'autre routeur
  set protocols ospfv3 redistribute connected
  ```
  <!--set protocols ospfv3 area 0 range 2001:0:8::/64 -->
- Date : `Fait` le 04/11/2024

**Contraintes 31 (1pt) :** La table de routage du routeur R2 dispose d’une route par défaut statique vers le réseau NAT/Cloud. La route par défaut est propagée en OSPF.
- Fait automatiquement en requestant une @IP dhcp sur l'interface outside
- Date : `Fait` le 03/11/2024

### 1.5 Les commutateurs

**Contraintes 32 (2pts) :** Le ou les commutateurs supportant les réseaux IP N10, N20 et N99 doivent être « pingables » depuis le terminal TA1.
- Chercher pour rendre Frugal :
  ```bash
  ip addr add 200.8.10.123/24 dev eth1
  ip addr add 200.8.10.124/24 dev eth2
  ip addr add 200.8.99.123/24 dev eth3
  ip addr add 200.8.99.124/24 dev eth4
  ip addr add 200.8.20.123/24 dev eth5
  ip addr add 200.8.20.124/24 dev eth6
  ``` 
- Date : `Fait` le 04/11/2024

**Contraintes 33 (5pts) :** Les réseaux N10, N20 et N99 sont isolés par des VLAN.
- Cf contraite 13
- Date : `Fait` le 03/11/2024

### 1.6 Le réseau Cloud

blank

## 2 Annexe

### 2.1 VyOS

#### 2.1.1 Serveur SSH

- Activer le serveur SSH :
```bash
vyos@Routeur# set service ssh
```
- Autoriser l’accès SSH à l’utilisateur vyos :
```bash
vyos@Routeur# set service ssh access-control allow user vyos
```

### 2.2 Protocole NAT

- Définir nom de l’interface externe eth0 (interface du réseau public) :
```bash
vyos@Routeur# set nat source rule 100 outbound-interface 'eth0'
```
- Définir la plage d’adresse 200.1.0.0/16 pouvant être translatée (réseau privé) :
```bash
set nat source rule 100 source address '200.1.0.0/16'
```
- Puis définir le mode de translation :
```bash
vyos@Routeur# set nat source rule 100 translation address masquerade
```

### 2.3 MicroCore

#### 2.3.1 Client SSH

- Installer dropbear
- Utiliser le client SSH pour accéder à l’équipement identifié par l’adresse d’interface 200.1.99.1, avec le compte utilisateur vyos :
```bash
tc@TA1:~$ dbclient vyos@200.1.99.1
```

#### 2.3.2 Sous-interface ethernet

- Créer la sous interface eth0.10 de l’interface physique eth0 associée au VLAN 10 (les paquets seront « taggués » en conséquence) :
```bash
tc@TS1:~$ sudo vconfig add eth0 10
```
- Activer l’interface physique et la sous-interface :
```bash
tc@TS1:~$ sudo ip link set dev eth0 up
tc@TS1:~$ sudo ip link set dev eth0.10 up
```

#### 2.3.3 Serveur DHCP

- Créer le fichier de configuration udhcpd.conf pour l’interface eth0.10 :
```bash
tc@TS1:~$ more /opt/udhcpd.conf
start 200.1.10.50                 ← 1ère adresse en bail
end 200.1.10.100                  ← dernière adresse en bail
interface eth0.10                 ← interface de réception des requêtes
option subnet 255.255.255.0       ← masque des adresses en bail
option router 200.1.10.1          ← adresse du routeur par défaut
```
- Activer l’interface physique et la sous-interface :
```bash
ttc@TS1:~$ udhcpd -f /opt/udhcpd.conf &
```
- Faire de même pour l’interface eth0.20.

### 2.3 Openvswitch

- Par défaut, les interfaces réseaux sont en mode trunk, et sont toutes associées au bridge br0 ;
- Configurer l’interface eth5 en mode access et l’associer au VLAN 99 :
```bash
/ # ovs-vsctl set port eth5 tag=99
```
- Créer et configurer l’interface VLAN 99 associée au VLAN 99 dans le bridge br0 :
```bash
/ # ovs-vsctl add-ports br0 vlan99 tag=99 – – bset interface vlan99 type=internal
```
- Assigner une adresse IPv4 à l’interface.

OSPF : https://docs.vyos.io/en/equuleus/configuration/protocols/ospf.html#ospfv3-ipv6
