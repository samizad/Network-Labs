# Configuration Steps

## Router RA Configuration
```cisco
interface Tunnel0
 ip address 10.10.10.1 255.255.255.252
 tunnel source Serial 0/0/0
 tunnel destination 209.165.122.2
 tunnel mode gre ip
 no shutdown

! Static route to Remote LAN via Tunnel
ip route 192.168.2.0 255.255.255.0 10.10.10.2

## Router RB Configuration
interface Tunnel0
 ip address 10.10.10.2 255.255.255.252
 tunnel source Serial0/0/0
 tunnel destination 64.103.211.2
 tunnel mode gre ip
 no shutdown

! Static route to Local LAN via Tunnel
ip route 192.168.1.0 255.255.255.0 10.10.10.1
