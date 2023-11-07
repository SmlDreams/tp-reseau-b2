# TP3 INFRA : Premiers pas GNS, Cisco et VLAN

# I. Dumb switch

## 3. Setup topologie 1

🌞 **Commençons simple**

- définissez les IPs statiques sur les deux VPCS :

```
PC1> ip 10.3.1.1/24
Checking for duplicate address...
PC1 : 10.3.1.1 255.255.255.0
```

- `ping` un VPCS depuis l'autre : 

```
PC1> ping 10.3.1.2

84 bytes from 10.3.1.2 icmp_seq=1 ttl=64 time=4.773 ms
84 bytes from 10.3.1.2 icmp_seq=2 ttl=64 time=5.264 ms
```


- afficher la CAM table du switch et vérifier les MAC qui s'y trouvent (Google pour ça, c'est pas dans le mémo) :

```
IOU14#show mac address-table
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----
   1    0050.7966.6800    DYNAMIC     Et0/0
   1    0050.7966.6801    DYNAMIC     Et0/1
Total Mac Addresses for this criterion: 2

```

# II. VLAN

### 3. Setup topologie 2

🌞 **Adressage**

- définissez les IPs statiques sur tous les VPCS : 

```
PC3> ip 10.3.1.3
Checking for duplicate address...
PC3 : 10.3.1.3 255.255.255.0
```

- vérifiez avec des `ping` que tout le monde se ping : 

```
PC3> ping 10.3.1.1

84 bytes from 10.3.1.1 icmp_seq=1 ttl=64 time=1.163 ms
84 bytes from 10.3.1.1 icmp_seq=2 ttl=64 time=2.857 ms

PC3> ping 10.3.1.2

84 bytes from 10.3.1.2 icmp_seq=1 ttl=64 time=4.366 ms
84 bytes from 10.3.1.2 icmp_seq=2 ttl=64 time=4.309 ms
```

🌞 **Configuration des VLANs**

- déclaration des VLANs sur le switch `sw1` :

```
IOU14(config)#vlan 10
IOU14(config-vlan)#exit
IOU14(config)#vlan 20
IOU14(config-vlan)#exit
```

- ajout des ports du switches dans le bon VLAN (voir [le tableau d'adressage de la topo 2 juste au dessus](#2-adressage-topologie-2))
  - ici, tous les ports sont en mode *access* : ils pointent vers des clients du réseau :

  ```
  IOU14#show vlan br

  VLAN Name                             Status    Ports
  ---- -------------------------------- --------- -------------------------------
  10   VLAN0010                         active    Et0/0, Et0/1
  20   VLAN0020                         active    Et0/2

  ```

🌞 **Vérif**

- `pc1` et `pc2` doivent toujours pouvoir se ping : 

  ```
  PC2> ping 10.3.1.1

  84 bytes from 10.3.1.1 icmp_seq=1 ttl=64 time=2.121 ms
  84 bytes from 10.3.1.1 icmp_seq=2 ttl=64 time=1.376 ms
  84 bytes from 10.3.1.1 icmp_seq=3 ttl=64 time=1.331 ms
  84 bytes from 10.3.1.1 icmp_seq=4 ttl=64 time=2.139 ms
  84 bytes from 10.3.1.1 icmp_seq=5 ttl=64 time=1.829 ms
  ```

- `pc3` ne ping plus personne :

  ```
  PC3> ping 10.3.1.2

  host (10.3.1.2) not reachable

  PC3> ping 10.3.1.1

  host (10.3.1.1) not reachable

  ```

# III. Ptite VM DHCP

🌞 **VM `dhcp.tp3.b2`**

- Rocky Linux, IP statique, nom défini à `dhcp.tp3.b2`, SELinux désactivé, firewall activé, système à jour, NORMAL LA ROUTINE QUOI
- installez un serveur DHCP : 

  ```
  [cquentin@DHCPtp3RESEAU ~]$ sudo dnf install dhcp-server
  ```

  - il doit délivrer des IPs entre `10.3.1.100` et `10.3.1.200` : 

    ```
    [cquentin@DHCPtp3RESEAU ~]$ sudo cat /etc/dhcp/dhcpd.conf
    #
    # DHCP Server Configuration file.
    #   see /usr/share/doc/dhcp-server/dhcpd.conf.example
    #   see dhcpd.conf(5) man page
    #

    # default lease time
    default-lease-time 600;
    # max lease time
    max-lease-time 7200;
    # this DHCP server to be declared valid
    authoritative;
    # specify network address and subnetmask
    subnet 10.3.1.0 netmask 255.255.255.0 {
        # specify the range of lease IP address
        range dynamic-bootp 10.3.1.100 10.3.1.200;
        # specify broadcast address
        option broadcast-address 10.3.1.255;
    }
    ```

- vérifier avec le `pc4` que vous pouvez récupérer une IP en DHCP :

    ```
    PC4> dhcp
    DDORA
    PC4> show ip IP 10.3.1.100/24

    NAME        : PC4[1]
    IP/MASK     : 10.3.1.100/24
    GATEWAY     : 0.0.0.0
    
    DHCP SERVER : 10.3.1.253
    DHCP LEASE  : 596, 600/300/525
    ```

- vérifier avec le `pc5` que vous ne pouvez PAS récupérer une IP en DHCP :

 ```
 PC5> dhcp
  DDD
  Can't find dhcp server
 ```
