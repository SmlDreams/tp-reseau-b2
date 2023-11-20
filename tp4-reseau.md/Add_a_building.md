# III. Add a building

## 3. Setup topologie 3

🌞  **Vous devez me rendre le `show running-config` de tous les équipements**

- de tous les équipements réseau
  - le routeur : 

    ```
        Current configuration : 1421 bytes
    !
    version 12.4
    service timestamps debug datetime msec
    service timestamps log datetime msec
    no service password-encryption
    !
    hostname R2
    !
    boot-start-marker
    boot-end-marker
    !
    !
    no aaa new-model
    memory-size iomem 5
    no ip icmp rate-limit unreachable
    ip cef
    !
    !
    !
    !
    no ip domain lookup
    !
    multilink bundle-name authenticated
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    archive
    log config
      hidekeys
    !
    !
    !
    !
    ip tcp synwait-time 5
    !
    !
    !
    !
    interface FastEthernet0/0
    no ip address
    ip nat inside
    ip virtual-reassembly
    duplex auto
    speed auto
    !
    interface FastEthernet0/0.10
    encapsulation dot1Q 10
    ip address 10.1.10.254 255.255.255.0
    !
    interface FastEthernet0/0.20
    encapsulation dot1Q 20
    ip address 10.1.20.254 255.255.255.0
    !
    interface FastEthernet0/0.30
    encapsulation dot1Q 30
    ip address 10.1.30.254 255.255.255.0
    !
    interface FastEthernet0/1
    ip address dhcp
    ip nat outside
    ip virtual-reassembly
    duplex auto
    speed auto
    !
    interface FastEthernet1/0
    no ip address
    shutdown
    duplex auto
    speed auto
    !
    interface FastEthernet2/0
    no ip address
    shutdown
    duplex auto
    speed auto
    !
    ip forward-protocol nd
    !
    !
    no ip http server
    no ip http secure-server
    !
    access-list 1 permit any
    no cdp log mismatch duplex
    !
    !
    !
    !
    !
    !
    control-plane
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    line con 0
    exec-timeout 0 0
    privilege level 15
    logging synchronous
    line aux 0
    exec-timeout 0 0
    privilege level 15
    logging synchronous
    line vty 0 4
    login
    !
    !
    end

    ```
  - les 3 switches :

    ```
    IOU2#show running-config
    Building configuration...

    Current configuration : 1688 bytes
    !
    ! Last configuration change at 12:25:47 UTC Mon Nov 20 2023
    !
    version 15.2
    service timestamps debug datetime msec
    service timestamps log datetime msec
    no service password-encryption
    service compress-config
    !
    hostname IOU2
    !
    boot-start-marker
    boot-end-marker
    !
    !
    logging discriminator EXCESS severity drops 6 msg-body drops EXCESSCOLL
    logging buffered 50000
    logging console discriminator EXCESS
    !
    no aaa new-model
    !
    !
    !
    !
    !
    no ip icmp rate-limit unreachable
    !
    !
    !
    no ip domain-lookup
    ip cef
    no ipv6 cef
    !
    !
    !
    spanning-tree mode pvst
    spanning-tree extend system-id
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    interface Ethernet0/0
    switchport access vlan 10
    switchport mode access
    !
    interface Ethernet0/1
    switchport access vlan 10
    switchport mode access
    !
    interface Ethernet0/2
    switchport access vlan 20
    switchport mode access
    !
    interface Ethernet0/3
    switchport access vlan 30
    switchport mode access
    !
    interface Ethernet1/0
    switchport trunk encapsulation dot1q
    switchport mode trunk
    !
    interface Ethernet1/1
    !
    interface Ethernet1/2
    !
    interface Ethernet1/3
    !
    interface Ethernet2/0
    !
    interface Ethernet2/1
    !
    interface Ethernet2/2
    !
    interface Ethernet2/3
    !
    interface Ethernet3/0
    !
    interface Ethernet3/1
    !
    interface Ethernet3/2
    !
    interface Ethernet3/3
    !
    interface Vlan1
    no ip address
    shutdown
    !
    ip forward-protocol nd
    !
    ip tcp synwait-time 5
    ip http server
    !
    ip ssh server algorithm encryption aes128-ctr aes192-ctr aes256-ctr
    ip ssh client algorithm encryption aes128-ctr aes192-ctr aes256-ctr
    !
    !
    !
    !
    !
    control-plane
    !
    !
    line con 0
    exec-timeout 0 0
    privilege level 15
    logging synchronous
    line aux 0
    exec-timeout 0 0
    privilege level 15
    logging synchronous
    line vty 0 4
    login
    !
    !
    !
    end
    ```

    ```
    IOU4#show run
    Building configuration...

    Current configuration : 1545 bytes
    !
    ! Last configuration change at 12:25:47 UTC Mon Nov 20 2023
    !
    version 15.2
    service timestamps debug datetime msec
    service timestamps log datetime msec
    no service password-encryption
    service compress-config
    !
    hostname IOU4
    !
    boot-start-marker
    boot-end-marker
    !
    !
    logging discriminator EXCESS severity drops 6 msg-body drops EXCESSCOLL
    logging buffered 50000
    logging console discriminator EXCESS
    !
    no aaa new-model
    !
    !
    !
    !
    !
    no ip icmp rate-limit unreachable
    !
    !
    !
    no ip domain-lookup
    ip cef
    no ipv6 cef
    !
    !
    !
    spanning-tree mode pvst
    spanning-tree extend system-id
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    interface Ethernet0/0
    switchport trunk encapsulation dot1q
    switchport mode trunk
    !
    interface Ethernet0/1
    !
    interface Ethernet0/2
    switchport trunk encapsulation dot1q
    switchport mode trunk
    !
    interface Ethernet0/3
    !
    interface Ethernet1/0
    !
    interface Ethernet1/1
    !
    interface Ethernet1/2
    !
    interface Ethernet1/3
    !
    interface Ethernet2/0
    !
    interface Ethernet2/1
    !
    interface Ethernet2/2
    !
    interface Ethernet2/3
    !
    interface Ethernet3/0
    !
    interface Ethernet3/1
    !
    interface Ethernet3/2
    !
    interface Ethernet3/3
    !
    interface Vlan1
    no ip address
    shutdown
    !
    --More--
    *Nov 20 12:47:52.181: %CDP-4-DUPLEX_MISMATCH: duplex mismatch discovered on Ethernet0/2 (not half duplex), with R2 FastEthernet0/0 (half duplex).
    ip forward-protocol nd
    !
    ip tcp synwait-time 5
    ip http server
    !
    ip ssh server algorithm encryption aes128-ctr aes192-ctr aes256-ctr
    ip ssh client algorithm encryption aes128-ctr aes192-ctr aes256-ctr
    !
    !
    !
    !
    !
    control-plane
    !
    !
    line con 0
    exec-timeout 0 0
    privilege level 15
    logging synchronous
    line aux 0
    exec-timeout 0 0
    privilege level 15
    logging synchronous
    line vty 0 4
    login
    !
    !
    !
    end
    ```

    ```
    IOU3#show run
    Building configuration...

    Current configuration : 1688 bytes
    !
    ! Last configuration change at 12:25:47 UTC Mon Nov 20 2023
    !
    version 15.2
    service timestamps debug datetime msec
    service timestamps log datetime msec
    no service password-encryption
    service compress-config
    !
    hostname IOU3
    !
    boot-start-marker
    boot-end-marker
    !
    !
    logging discriminator EXCESS severity drops 6 msg-body drops EXCESSCOLL
    logging buffered 50000
    logging console discriminator EXCESS
    !
    no aaa new-model
    !
    !
    !
    !
    !
    no ip icmp rate-limit unreachable
    !
    !
    !
    no ip domain-lookup
    ip cef
    no ipv6 cef
    !
    !
    !
    spanning-tree mode pvst
    spanning-tree extend system-id
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    !
    interface Ethernet0/0
    switchport access vlan 10
    switchport mode access
    !
    interface Ethernet0/1
    switchport access vlan 10
    switchport mode access
    !
    interface Ethernet0/2
    switchport access vlan 10
    switchport mode access
    !
    interface Ethernet0/3
    switchport access vlan 10
    switchport mode access
    !
    interface Ethernet1/0
    switchport trunk encapsulation dot1q
    switchport mode trunk
    !
    interface Ethernet1/1
    !
    interface Ethernet1/2
    !
    interface Ethernet1/3
    !
    interface Ethernet2/0
    !
    interface Ethernet2/1
    !
    interface Ethernet2/2
    !
    interface Ethernet2/3
    !
    interface Ethernet3/0
    !
    interface Ethernet3/1
    !
    interface Ethernet3/2
    !
    interface Ethernet3/3
    !
    interface Vlan1
    no ip address
    shutdown
    !
    ip forward-protocol nd
    !
    ip tcp synwait-time 5
    ip http server
    !
    ip ssh server algorithm encryption aes128-ctr aes192-ctr aes256-ctr
    ip ssh client algorithm encryption aes128-ctr aes192-ctr aes256-ctr
    !
    !
    !
    !
    !
    control-plane
    !
    !
    line con 0
    exec-timeout 0 0
    privilege level 15
    logging synchronous
    line aux 0
    exec-timeout 0 0
    privilege level 15
    logging synchronous
    line vty 0 4
    login
    !
    !
    !
    end


    ```

> N'oubliez pas les VLANs sur tous les switches.

🖥️ **VM `dhcp1.client1.tp4`**, déroulez la [Checklist VM Linux](#checklist-vm-linux) dessus

🌞  **Mettre en place un serveur DHCP dans le nouveau bâtiment**

```
[cquentin@DHCPtp3RESEAU ~]$ sudo cat /etc/dhcp/dhcpd.conf

default-lease-time 600;

max-lease-time 7200;

authoritative;

subnet 10.1.10.0 netmask 255.255.255.0 {
   
    range dynamic-bootp 10.1.10.100 10.1.10.200;
    
    option broadcast-address 10.1.10.255;

    option domain-name-servers 1.1.1.1;

    option routers 10.1.10.254;
}
```

🌞  **Vérification**

- un client récupère une IP en DHCP : 
  ```
  PC3> dhcp
  DDORA IP 10.1.10.100/24 GW 10.1.10.254
  ```

- il peut ping le serveur Web :

  ```
  PC3> ping 10.1.30.46

  84 bytes from 10.1.30.46 icmp_seq=1 ttl=63 time=20.175 ms
  84 bytes from 10.1.30.46 icmp_seq=2 ttl=63 time=34.943 ms
  ```

- il peut ping `1.1.1.1` :

  ```
  PC3> ping 1.1.1.1

  84 bytes from 1.1.1.1 icmp_seq=1 ttl=55 time=43.157 ms
  84 bytes from 1.1.1.1 icmp_seq=2 ttl=55 time=42.521 ms
  84 bytes from 1.1.1.1 icmp_seq=3 ttl=55 time=43.599 ms
  84 bytes from 1.1.1.1 icmp_seq=4 ttl=55 time=41.035 ms
  84 bytes from 1.1.1.1 icmp_seq=5 ttl=55 time=45.630 ms
  ```

- il peut ping `ynov.com` :

  ```
  PC3> ping ynov.com
  ynov.com resolved to 172.67.74.226

  84 bytes from 172.67.74.226 icmp_seq=1 ttl=55 time=41.671 ms
  84 bytes from 172.67.74.226 icmp_seq=2 ttl=55 time=45.919 ms
  84 bytes from 172.67.74.226 icmp_seq=3 ttl=55 time=36.996 ms
  84 bytes from 172.67.74.226 icmp_seq=4 ttl=55 time=43.056 ms
  84 bytes from 172.67.74.226 icmp_seq=5 ttl=55 time=42.921 ms
  ```
