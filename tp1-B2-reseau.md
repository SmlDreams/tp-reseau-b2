# TP1 : Maîtrise réseau du poste

# I. Basics

☀️ **Carte réseau WiFi**

- l'adresse MAC de votre carte WiFi
- l'adresse IP de votre carte WiFi
- le masque de sous-réseau du réseau LAN auquel vous êtes connectés en WiFi
  - en notation CIDR, par exemple `/16`
  - ET en notation décimale, par exemple `255.255.0.0`

```
PS C:\Users\quentin> ipconfig

Carte réseau sans fil Wi-Fi :

   Suffixe DNS propre à la connexion. . . :
   Adresse IPv6 de liaison locale. . . . .: fe80::148d:747f:5cb7:c5b3%23
   Adresse IPv4. . . . . . . . . . . . . .: 10.33.67.35
   Masque de sous-réseau. . . . . . . . . : 255.255.240.0
```

le masque de sous réseau en notation CIDR s'obtient en convertissant chaque nombre du masque de sous réseau en binaire et en comptant le nombre de total de 1.Donc ici /20.


☀️ **Déso pas déso**

- l'adresse de réseau du LAN auquel vous êtes connectés en WiFi : 

10.33.67.0

- l'adresse de broadcast

10.33.67.254

- le nombre d'adresses IP disponibles dans ce réseau

---

☀️ **Hostname**

- déterminer le hostname de votre PC

---

☀️ **Passerelle du réseau**

Déterminer...

- l'adresse IP de la passerelle du réseau
- l'adresse MAC de la passerelle du réseau

---

☀️ **Serveur DHCP et DNS**

Déterminer...

- l'adresse IP du serveur DHCP qui vous a filé une IP
- l'adresse IP du serveur DNS que vous utilisez quand vous allez sur internet

---

☀️ **Table de routage**

Déterminer...

- dans votre table de routage, laquelle est la route par défaut

---


☀️ **Hosts ?**

- faites en sorte que pour votre PC, le nom `b2.hello.vous` corresponde à l'IP `1.1.1.1`:

```
PS C:\Windows\System32\drivers\etc> cat .\hosts

1.1.1.1 b2.hello.vous
```

- prouvez avec un `ping b2.hello.vous` que ça ping bien `1.1.1.1` : 

```
PS C:\Windows\System32\drivers\etc> ping b2.hello.vous

Envoi d’une requête 'ping' sur b2.hello.vous [1.1.1.1] avec 32 octets de données :
Réponse de 1.1.1.1 : octets=32 temps=10 ms TTL=57

Statistiques Ping pour 1.1.1.1:
    Paquets : envoyés = 3, reçus = 3, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 10ms, Maximum = 13ms, Moyenne = 11ms
```


☀️ **Go mater une vidéo youtube et déterminer, pendant qu'elle tourne...**

```
PS C:\WINDOWS\system32> netstat -anb | Select-String 216.58.214.174 -context 0,1

UDP    0.0.0.0:64957          216.58.214.174:443
   [brave.exe]
```

le port du server est 443 et le port de ma machine est 64957.

☀️ **Requêtes DNS**

- à quelle adresse IP correspond le nom de domaine `www.ynov.com`:

```
PS C:\WINDOWS\system32> nslookup www.ynov.com

Réponse ne faisant pas autorité :
Nom :    www.ynov.com
Addresses:  2606:4700:20::ac43:4ae2
          2606:4700:20::681a:be9
          2606:4700:20::681a:ae9
          104.26.11.233
          172.67.74.226
          104.26.10.233
```

- à quel nom de domaine correspond l'IP `174.43.238.89` :

```
PS C:\WINDOWS\system32> nslookup 174.43.238.89

Nom :    89.sub-174-43-238.myvzw.com
Address:  174.43.238.89
```

☀️ **Hop hop hop**

Déterminer...

- par combien de machines vos paquets passent quand vous essayez de joindre `www.ynov.com` :

```
PS C:\WINDOWS\system32> tracert www.ynov.com

Détermination de l’itinéraire vers www.ynov.com [172.67.74.226]
avec un maximum de 30 sauts :

  1     2 ms     7 ms     3 ms  10.33.79.254
  2     4 ms     4 ms     3 ms  145.117.7.195.rev.sfr.net [195.7.117.145]
  3     6 ms     4 ms     3 ms  237.195.79.86.rev.sfr.net [86.79.195.237]
  4     4 ms     4 ms     5 ms  196.224.65.86.rev.sfr.net [86.65.224.196]
  5    15 ms    13 ms    13 ms  12.148.6.194.rev.sfr.net [194.6.148.12]
  6    12 ms    11 ms    10 ms  12.148.6.194.rev.sfr.net [194.6.148.12]
  7    19 ms    24 ms    11 ms  141.101.67.48
  8    12 ms    12 ms    11 ms  172.71.124.4
  9    12 ms    11 ms    11 ms  172.67.74.226
```

☀️ **IP publique**

Déterminer...

- l'adresse IP publique de la passerelle du réseau (le routeur d'YNOV donc si vous êtes dans les locaux d'YNOV quand vous faites le TP) :

```
PS C:\WINDOWS\system32> curl ifconfig.me.

Content           : 195.7.117.146
```

☀️ **Scan réseau**

Déterminer...

- combien il y a de machines dans le LAN auquel vous êtes connectés

```
PS C:\Users\quentin> nmap -sn 10.33.64.0/20

Nmap done: 4096 IP addresses (919 hosts up) scanned in 177.03 seconds
```

# III. Le requin

☀️ **Capture ARP**

```
PS C:\Users\quentin> arp -d
```
```markdown
[Capture arp](./captures/arp.pcap)
```

j'ai utilisé le filtre "arp" dans wireshark.

☀️ **Capture DNS**


```
PS C:\Users\quentin>  ping 8.8.8.8

Envoi d’une requête 'Ping'  8.8.8.8 avec 32 octets de données :
Réponse de 8.8.8.8 : octets=32 temps=22 ms TTL=113

Statistiques Ping pour 8.8.8.8:
    Paquets : envoyés = 3, reçus = 3, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 22ms, Maximum = 23ms, Moyenne = 22ms
```
```markdown
[Capture dns](./captures/dns.pcap)
```

j'ai utilisé le filtre "dns" dans wireshark.

☀️ **Capture TCP**

```markdown
[Capture tcp](./captures/tcp.pcap)
```

j'ai utilisé le filtre "tcp" dans wireshark.