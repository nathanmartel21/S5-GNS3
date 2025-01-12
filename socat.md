### Etablir un flux multicast :


1) **Sur le serveur** : 

```
socat -u STDIO UDP4-DATAGRAM:224.1.0.1:5000,ip-add-membership=224.1.0.1:eth0
```

2) ***Sur les clients** :

```
socat -u UDP4-RECVFROM:5000,ip-add-membership=224.1.0.1:eth0 STDOUT
```

3) **Sur le serveur** :

```
Bonjour
```

4) **Sur les clients** :

Le message "bonjour" devrait apparaître sur les clients.
