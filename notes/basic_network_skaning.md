## ip

modern Linux network management tool
### addr

shows network interfaces and their assigned  IP addresses

```cli
ip addr

>>> output

1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:2f:0c:41 brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic noprefixroute eth0
       valid_lft 387sec preferred_lft 387sec
    inet6 fe80::9829:8de9:530d:9740/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:dd:89:87 brd ff:ff:ff:ff:ff:ff
    inet 192.168.56.101/24 brd 192.168.56.255 scope global dynamic noprefixroute eth1
       valid_lft 387sec preferred_lft 387sec
    inet6 fe80::a00:27ff:fedd:8987/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
```

`addr` — show interface addresses
`-br` — breaf, short format 

```cli
ip -br addr

>>> output

lo               UNKNOWN        127.0.0.1/8 ::1/128 
eth0             UP             10.0.2.15/24 fe80::9829:8de9:530d:9740/64 
eth1             UP             192.168.56.101/24 fe80::a00:27ff:fedd:8987/64 
```

**which means:**

`lo` — loopback, connecting the computer to itself
`eth0`,  `eth1` — network interfaces
`UP` — interface is enabled
`10.0.2.15/24` — Kali's address in the NAT network
`192.168.56.101/24` — Kali's address in the Host-only network

**show only one interface:**

```cli
ip addr show eth1

>>> output

3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:dd:89:87 brd ff:ff:ff:ff:ff:ff
    inet 192.168.56.101/24 brd 192.168.56.255 scope global dynamic noprefixroute eth1
       valid_lft 325sec preferred_lft 325sec
    inet6 fe80::a00:27ff:fedd:8987/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
```
### ip route

shows the routing table — where Linux sends packets.

```cli
ip route

>>> output

default via 10.0.2.1 dev eth0 proto dhcp src 10.0.2.15 metric 101 
10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15 metric 101 
192.168.56.0/24 dev eth1 proto kernel scope link src 192.168.56.101 metric 100 
```

`default` — the rout for all unknown networks, usualy the internet
`via 10.0.2.1` — send via geteway 10.0.2.1
`dev eth0`— use the eth0 interface

**line:**

`192.168.56.0/24 dev eth1`

**mean:**

the 192.168.56.0/24 network is connected directly via eth1

**other designations:**

`proto kernl` — the route was automatically created by the Linux kernel
`scope link` — the network is accessible, without a gateway
`src 192.168.56.101` — Kali will use this source address
`metric` — rout priority: a lower number usualy means a higher priority
### ip neigh

shows a tables of neighbors — devices, that thecomputer has recently interacted with on the local network

```cli
ip neigh

>>> output

192.168.56.100 dev eth1 lladdr 08:00:27:df:6d:d9 STALE 
10.0.2.1 dev eth0 lladdr 52:54:00:12:35:00 STALE 
10.0.2.2 dev eth0 lladdr 08:00:27:b6:f0:30 STALE 
```

**decoding:**

`192.168.56.100` — IP of the neighboring device
`dev eth1` — the neighboting device is accessible via eth1
`lladdr` — channel address
`08:00:27:df:6d:d9` — the MAC address of the device 
`STALE` — the entry is outdated
### ping -c 4 \<IP target\>

can Kali exchange packets with specific device

```cli
ping -c 4 192.168.56.100

>>> output

PING 192.168.56.100 (192.168.56.100) 56(84) bytes of data.
64 bytes from 192.168.56.100: icmp_seq=1 ttl=255 time=0.493 ms
64 bytes from 192.168.56.100: icmp_seq=2 ttl=255 time=0.217 ms
64 bytes from 192.168.56.100: icmp_seq=3 ttl=255 time=0.301 ms
64 bytes from 192.168.56.100: icmp_seq=4 ttl=255 time=0.223 ms

--- 192.168.56.100 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3076ms
rtt min/avg/max/mdev = 0.217/0.308/0.493/0.111 ms
```

**parametrs:**

`ping` — sends an ICMP Echo Request
`-c 4` — send four requests and complete
### nmap

searches for active devices on the specified network without scaning ports

```cli
nmap -sn 192.168.56.0/24

>>> output

Starting Nmap 7.99 ( https://nmap.org ) at 2026-09-28 12:53 -0400
Nmap scan report for 192.168.56.1
Host is up (0.00028s latency).
MAC Address: 0A:00:27:00:00:07 (Unknown)
Nmap scan report for 192.168.56.100
Host is up (0.00031s latency).
MAC Address: 08:00:27:DF:6D:D9 (Oracle VirtualBox virtual NIC)
Nmap scan report for 192.168.56.102
Host is up (0.00066s latency).
MAC Address: 08:00:27:F9:FC:35 (Oracle VirtualBox virtual NIC)
Nmap scan report for 192.168.56.101
Host is up.
Nmap done: 256 IP addresses (4 hosts up) scanned in 7.30 seconds
```

`sn` — perform host detection only and skip port scanning
