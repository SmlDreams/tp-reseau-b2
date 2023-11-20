# I. Topo 1 : VLAN et Routing

## 3. Setup topologie 1

🖥️ VM `web1.servers.tp4`, déroulez la [Checklist VM Linux](#checklist-vm-linux) dessus

🌞 **Adressage**

- définissez les IPs statiques sur toutes les machines **sauf le *routeur*** : 

    ```
    PC1> ip 10.1.10.1
    Checking for duplicate address...
    PC1 : 10.1.10.1 255.255.255.0

    PC2> ip 10.1.10.2
    Checking for duplicate address...
    PC2 : 10.1.10.2 255.255.255.0

    Admin> ip 10.1.20.1
    Checking for duplicate address...
    Admin : 10.1.20.1 255.255.255.0
    ```

🌞 **Configuration des VLANs**

- déclaration des VLANs sur le switch `sw1` :

    ```
    IOU1(config)#vlan 10
    IOU1(config-vlan)#name clients
    IOU1(config-vlan)#exit
    IOU1(config)#vlan 20
    IOU1(config-vlan)#name admins
    IOU1(config-vlan)#exit
    IOU1(config)#vlan 30
    IOU1(config-vlan)#name servers
    IOU1(config-vlan)#exit
    ```

- ajout des ports du switches dans le bon VLAN : 

    ```
    IOU1#show vlan br

    VLAN Name                             Status    Ports
    ---- -------------------------------- --------- -------------------------------
    1    default                          active    Et1/0, Et1/1, Et1/2, Et1/3
                                                    Et2/0, Et2/1, Et2/2, Et2/3
                                                    Et3/0, Et3/1, Et3/2, Et3/3
    10   clients                          active    Et0/0, Et0/1
    20   admins                           active    Et0/2
    30   servers                          active    Et0/3

    IOU1(config)#int Et3/3
    IOU1(config-if)#sw trunk encap dot1q
    IOU1(config-if)#sw m t
    IOU1(config-if)#sw t a v add 10,20,30
    IOU1(config-if)#exit
    IOU1(config)#exit
    IOU1#show int t

    Port        Vlans allowed and active in management domain
    Et3/3       1,10,20,30
    ```

🌞 **Config du *routeur***

- attribuez ses IPs au *routeur* :

    ```
    R1(config)#int fastE0/0.10
    R1(config-subif)#encap dot1Q 10
    R1(config-subif)#ip addr 10.1.10.254 255.255.255.0
    R1(config-subif)#exit
    R1(config)#int fastE0/0.20
    R1(config-subif)#encap dot1Q 20
    R1(config-subif)#ip addr 10.1.20.254 255.255.255.0
    R1(config-subif)#exit
    R1(config)#int fastE0/0.30
    R1(config-subif)#encap dot1Q 30
    R1(config-subif)#ip addr 10.1.30.254 255.255.255.0
    ```

🌞 **Vérif**

- tout le monde doit pouvoir ping le routeur sur l'IP qui est dans son réseau :

  ```
  #vpcs-client

  PC1> ping 10.1.10.254

  10.1.10.254 icmp_seq=1 timeout
  84 bytes from 10.1.10.254 icmp_seq=2 ttl=255 time=10.963 ms


  #web-tp4

  [cquentin@web-server-tp4 ~]$ ping 10.1.30.254
  PING 10.1.30.254 (10.1.30.254) 56(84) bytes of data.
  64 bytes from 10.1.30.254: icmp_seq=1 ttl=255 time=14.7 ms

  --- 10.1.30.254 ping statistics ---
  2 packets transmitted, 2 received, 0% packet loss, time 1002ms
  rtt min/avg/max/mdev = 10.354/12.517/14.681/2.163 ms
  ```

- en ajoutant une route vers les réseaux, ils peuvent se ping entre eux :
  
  ```
  [cquentin@web-server-tp4 ~]$ sudo ip route add default via 10.1.30.254 dev enp0s3


  PC1> ip 10.1.10.1/24 10.1.10.254
  Checking for duplicate address...
  PC1 : 10.1.10.1 255.255.255.0 gateway 10.1.10.254



  PC1> ping 10.1.30.1

  84 bytes from 10.1.30.1 icmp_seq=1 ttl=63 time=20.451 ms
  84 bytes from 10.1.30.1 icmp_seq=2 ttl=63 time=17.165 ms

  ```
