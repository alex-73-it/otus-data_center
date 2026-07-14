# Адресное пространство и схема сети
![](<Схема сети.png>)
# Конфигурации устройств
<details>
<summary> Конфиг Leaf_1 </summary>

```
key chain bgp tcp
  key 0
    send-id 0
    recv-id 0
    key-string bgp
    send-lifetime local 00:00:00 Jan 01 2026  infinite
    cryptographic-algorithm HMAC-SHA-256

feature bgp

router bgp 65011
  router-id 10.10.10.0
  bestpath as-path multipath-relax
  log-neighbor-changes
  address-family ipv4 unicast
    network 10.10.10.0/32
    network 10.13.11.0/24
    maximum-paths 2
  template peer TEMPL_BGP_SPINES
    remote-as 65000
    ao bgp
    timers 10 30
    address-family ipv4 unicast
  neighbor 10.12.11.1
    inherit peer TEMPL_BGP_SPINES
  neighbor 10.12.12.1
    inherit peer TEMPL_BGP_SPINES
```
</details>

<details>
<summary> Конфиг Leaf_2 </summary>

```
key chain bgp tcp
  key 0
    send-id 0
    recv-id 0
    key-string bgp
    send-lifetime local 00:00:00 Jan 01 2026  infinite
    cryptographic-algorithm HMAC-SHA-256

feature bgp

router bgp 65012
  router-id 10.10.20.0
  bestpath as-path multipath-relax
  log-neighbor-changes
  address-family ipv4 unicast
    network 10.10.20.0/32
    network 10.13.22.0/24
    maximum-paths 2
  template peer TEMPL_BGP_SPINES
    remote-as 65000
    password 6 JDYkOzMRvLLW7v95qQYwdzJIKSTCSOKRLSCUxmVFiH8BTwfJJIkdcW0r59rWKkmRxuwWLBwat8kfAA==
    ao bgp
    timers 10 30
    address-family ipv4 unicast
  neighbor 10.12.21.1
    inherit peer TEMPL_BGP_SPINES
  neighbor 10.12.22.1
    inherit peer TEMPL_BGP_SPINES
```
</details>

<details>
<summary> Конфиг Leaf_3 </summary>

```
key chain bgp tcp
  key 0
    send-id 0
    recv-id 0
    key-string bgp
    send-lifetime local 00:00:00 Jan 01 2026  infinite
    cryptographic-algorithm HMAC-SHA-256

feature bgp

router bgp 65013
  router-id 10.10.30.0
  bestpath as-path multipath-relax
  log-neighbor-changes
  address-family ipv4 unicast
    network 10.10.30.0/32
    network 10.13.33.0/24
    network 10.13.34.0/24
    maximum-paths 2
  template peer TEMPL_BGP_SPINES
    remote-as 65000
    ao bgp
    timers 10 30
    address-family ipv4 unicast
  neighbor 10.12.31.1
    inherit peer TEMPL_BGP_SPINES
  neighbor 10.12.32.1
    inherit peer TEMPL_BGP_SPINES
```
</details>

<details>
<summary> Конфиг Spine_1 </summary>

```
key chain bgp tcp
  key 0
    send-id 0
    recv-id 0
    key-string bgp
    send-lifetime local 00:00:00 Jan 01 2026  infinite
    cryptographic-algorithm HMAC-SHA-256

feature bgp

router bgp 65000
  router-id 10.10.1.0
  bestpath as-path multipath-relax
  reconnect-interval 12
  log-neighbor-changes
  address-family ipv4 unicast
    network 10.10.1.0/32
    maximum-paths 2
  template peer TEMPL_BGP_LEAVES
    password 3 f9ae547b68602ed9
    ao bgp
    timers 10 30
    address-family ipv4 unicast
  neighbor 10.12.0.0/16 remote-as route-map RM_AS_LEAVES
    inherit peer TEMPL_BGP_LEAVES
```
</details>

<details>
<summary> Конфиг Spine_2 </summary>

```
key chain bgp tcp
  key 0
    send-id 0
    recv-id 0
    key-string bgp
    send-lifetime local 00:00:00 Jan 01 2026  infinite
    cryptographic-algorithm HMAC-SHA-256

feature bgp

router bgp 65000
  router-id 10.10.2.0
  bestpath as-path multipath-relax
  reconnect-interval 12
  log-neighbor-changes
  address-family ipv4 unicast
    network 10.10.2.0/32
    maximum-paths 2
  template peer TEMPL_BGP_LEAVES
    ao bgp
    timers 10 30
    address-family ipv4 unicast
  neighbor 10.12.0.0/16 remote-as route-map RM_AS_LEAVES
    inherit peer TEMPL_BGP_LEAVES
```
</details>

# Проверка работоспособности
1. Вывод команд show на устройствах
<details> 
<summary> Leaf_1 </summary>

```
Leaf_1# sh ip bgp summary
BGP summary information for VRF default, address family IPv4 Unicast
BGP router identifier 10.10.10.0, local AS number 65011
BGP table version is 114, IPv4 Unicast config peers 2, capable peers 2
9 network entries and 14 paths using 3796 bytes of memory
BGP attribute entries [4/1472], BGP AS path entries [3/26]
BGP community entries [0/0], BGP clusterlist entries [0/0]

Neighbor        V    AS    MsgRcvd    MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
10.12.11.1      4 65000      10113      10095      114    0    0 03:07:37 6
10.12.12.1      4 65000       9619       9606      114    0    0 01:52:47 6
Leaf_1# sh ip bgp
BGP routing table information for VRF default, address family IPv4 Unicast
BGP table version is 114, Local Router ID is 10.10.10.0
Status: s-suppressed, x-deleted, S-stale, d-dampened, h-history, *-valid, >-best
Path type: i-internal, e-external, c-confed, l-local, a-aggregate, r-redist, I-injected
Origin codes: i - IGP, e - EGP, ? - incomplete, | - multipath, & - backup, 2 - best2

   Network            Next Hop            Metric     LocPrf     Weight Path
*>e10.10.1.0/32       10.12.11.1                                     0 65000 i
*>e10.10.2.0/32       10.12.12.1                                     0 65000 i
*>l10.10.10.0/32      0.0.0.0                           100      32768 i
*|e10.10.20.0/32      10.12.11.1                                     0 65000 65012 i
*>e                   10.12.12.1                                     0 65000 65012 i
*|e10.10.30.0/32      10.12.11.1                                     0 65000 65013 i
*>e                   10.12.12.1                                     0 65000 65013 i
*>l10.13.11.0/24      0.0.0.0                           100      32768 i
*|e10.13.22.0/24      10.12.11.1                                     0 65000 65012 i
*>e                   10.12.12.1                                     0 65000 65012 i
*|e10.13.33.0/24      10.12.11.1                                     0 65000 65013 i
*>e                   10.12.12.1                                     0 65000 65013 i
*|e10.13.34.0/24      10.12.11.1                                     0 65000 65013 i
*>e                   10.12.12.1                                     0 65000 65013 i
```
</details>

<details> 
<summary> Leaf_2 </summary>

```
Leaf_2# sh ip bgp summary
BGP summary information for VRF default, address family IPv4 Unicast
BGP router identifier 10.10.20.0, local AS number 65012
BGP table version is 94, IPv4 Unicast config peers 2, capable peers 2
9 network entries and 14 paths using 3796 bytes of memory
BGP attribute entries [4/1472], BGP AS path entries [3/26]
BGP community entries [0/0], BGP clusterlist entries [0/0]

Neighbor        V    AS    MsgRcvd    MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
10.12.21.1      4 65000      10038      10029       94    0    0 02:04:58 6
10.12.22.1      4 65000      10339      10329       94    0    0 01:54:42 6
Leaf_2# sh ip bgp
BGP routing table information for VRF default, address family IPv4 Unicast
BGP table version is 94, Local Router ID is 10.10.20.0
Status: s-suppressed, x-deleted, S-stale, d-dampened, h-history, *-valid, >-best
Path type: i-internal, e-external, c-confed, l-local, a-aggregate, r-redist, I-injected
Origin codes: i - IGP, e - EGP, ? - incomplete, | - multipath, & - backup, 2 - best2

   Network            Next Hop            Metric     LocPrf     Weight Path
*>e10.10.1.0/32       10.12.21.1                                     0 65000 i
*>e10.10.2.0/32       10.12.22.1                                     0 65000 i
*|e10.10.10.0/32      10.12.21.1                                     0 65000 65011 i
*>e                   10.12.22.1                                     0 65000 65011 i
*>l10.10.20.0/32      0.0.0.0                           100      32768 i
*|e10.10.30.0/32      10.12.21.1                                     0 65000 65013 i
*>e                   10.12.22.1                                     0 65000 65013 i
*|e10.13.11.0/24      10.12.21.1                                     0 65000 65011 i
*>e                   10.12.22.1                                     0 65000 65011 i
*>l10.13.22.0/24      0.0.0.0                           100      32768 i
*|e10.13.33.0/24      10.12.21.1                                     0 65000 65013 i
*>e                   10.12.22.1                                     0 65000 65013 i
*|e10.13.34.0/24      10.12.21.1                                     0 65000 65013 i
*>e                   10.12.22.1                                     0 65000 65013 i
```
</details>

<details> 
<summary> Leaf_3 </summary>

```
Leaf_3# sh ip bgp summary
BGP summary information for VRF default, address family IPv4 Unicast
BGP router identifier 10.10.30.0, local AS number 65013
BGP table version is 70, IPv4 Unicast config peers 2, capable peers 2
9 network entries and 13 paths using 3692 bytes of memory
BGP attribute entries [4/1472], BGP AS path entries [3/26]
BGP community entries [0/0], BGP clusterlist entries [0/0]

Neighbor        V    AS    MsgRcvd    MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
10.12.31.1      4 65000       9630       9618       70    0    0 01:47:14 5
10.12.32.1      4 65000       9819       9821       70    0    0 01:47:15 5
Leaf_3# sh ip bgp
BGP routing table information for VRF default, address family IPv4 Unicast
BGP table version is 70, Local Router ID is 10.10.30.0
Status: s-suppressed, x-deleted, S-stale, d-dampened, h-history, *-valid, >-best
Path type: i-internal, e-external, c-confed, l-local, a-aggregate, r-redist, I-injected
Origin codes: i - IGP, e - EGP, ? - incomplete, | - multipath, & - backup, 2 - best2

   Network            Next Hop            Metric     LocPrf     Weight Path
*>e10.10.1.0/32       10.12.31.1                                     0 65000 i
*>e10.10.2.0/32       10.12.32.1                                     0 65000 i
*|e10.10.10.0/32      10.12.31.1                                     0 65000 65011 i
*>e                   10.12.32.1                                     0 65000 65011 i
*|e10.10.20.0/32      10.12.31.1                                     0 65000 65012 i
*>e                   10.12.32.1                                     0 65000 65012 i
*>l10.10.30.0/32      0.0.0.0                           100      32768 i
*|e10.13.11.0/24      10.12.31.1                                     0 65000 65011 i
*>e                   10.12.32.1                                     0 65000 65011 i
*|e10.13.22.0/24      10.12.31.1                                     0 65000 65012 i
*>e                   10.12.32.1                                     0 65000 65012 i
*>l10.13.33.0/24      0.0.0.0                           100      32768 i
*>l10.13.34.0/24      0.0.0.0                           100      32768 i
```
</details>

<details> 
<summary> Spine_1 </summary>

```
Spine_1# sh ip bgp summary
BGP summary information for VRF default, address family IPv4 Unicast
BGP router identifier 10.10.1.0, local AS number 65000
BGP table version is 70, IPv4 Unicast config peers 4, capable peers 3
8 network entries and 8 paths using 2912 bytes of memory
BGP attribute entries [4/1472], BGP AS path entries [3/18]
BGP community entries [0/0], BGP clusterlist entries [0/0]

Neighbor        V    AS    MsgRcvd    MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
10.12.11.0      4 65011       1151       1149       70    0    0 03:10:57 2
10.12.21.0      4 65012        764        762       70    0    0 02:06:23 2
10.12.31.0      4 65013        652        651       70    0    0 01:47:49 3
Spine_1# sh ip bgp
BGP routing table information for VRF default, address family IPv4 Unicast
BGP table version is 70, Local Router ID is 10.10.1.0
Status: s-suppressed, x-deleted, S-stale, d-dampened, h-history, *-valid, >-best
Path type: i-internal, e-external, c-confed, l-local, a-aggregate, r-redist, I-injected
Origin codes: i - IGP, e - EGP, ? - incomplete, | - multipath, & - backup, 2 - best2

   Network            Next Hop            Metric     LocPrf     Weight Path
*>l10.10.1.0/32       0.0.0.0                           100      32768 i
*>e10.10.10.0/32      10.12.11.0                                     0 65011 i
*>e10.10.20.0/32      10.12.21.0                                     0 65012 i
*>e10.10.30.0/32      10.12.31.0                                     0 65013 i
*>e10.13.11.0/24      10.12.11.0                                     0 65011 i
*>e10.13.22.0/24      10.12.21.0                                     0 65012 i
*>e10.13.33.0/24      10.12.31.0                                     0 65013 i
*>e10.13.34.0/24      10.12.31.0                                     0 65013 i
```
</details>

<details> 
<summary> Spine_2 </summary>

```
Spine_2# sh ip bgp summary
BGP summary information for VRF default, address family IPv4 Unicast
BGP router identifier 10.10.2.0, local AS number 65000
BGP table version is 112, IPv4 Unicast config peers 4, capable peers 3
8 network entries and 8 paths using 2912 bytes of memory
BGP attribute entries [4/1472], BGP AS path entries [3/18]
BGP community entries [0/0], BGP clusterlist entries [0/0]

Neighbor        V    AS    MsgRcvd    MsgSent   TblVer  InQ OutQ Up/Down  State/
PfxRcd
10.12.12.0      4 65011        706        705      112    0    0 01:56:50 2

10.12.22.0      4 65012        706        705      112    0    0 01:56:50 2

10.12.32.0      4 65013        657        656      112    0    0 01:48:32 3

Spine_2# sh ip bgp
BGP routing table information for VRF default, address family IPv4 Unicast
BGP table version is 112, Local Router ID is 10.10.2.0
Status: s-suppressed, x-deleted, S-stale, d-dampened, h-history, *-valid, >-best
Path type: i-internal, e-external, c-confed, l-local, a-aggregate, r-redist, I-i
njected
Origin codes: i - IGP, e - EGP, ? - incomplete, | - multipath, & - backup, 2 - b
est2

   Network            Next Hop            Metric     LocPrf     Weight Path
*>l10.10.2.0/32       0.0.0.0                           100      32768 i
*>e10.10.10.0/32      10.12.12.0                                     0 65011 i
*>e10.10.20.0/32      10.12.22.0                                     0 65012 i
*>e10.10.30.0/32      10.12.32.0                                     0 65013 i
*>e10.13.11.0/24      10.12.12.0                                     0 65011 i
*>e10.13.22.0/24      10.12.22.0                                     0 65012 i
*>e10.13.33.0/24      10.12.32.0                                     0 65013 i
*>e10.13.34.0/24      10.12.32.0                                     0 65013 i
```
</details>

2. Проверка доступности сетей c помощью ping
<details>
<summary> ping с PC_1 до осатльных PC </summary>

```
pc_1> ping 10.13.22.2

84 bytes from 10.13.22.2 icmp_seq=1 ttl=61 time=23.920 ms
84 bytes from 10.13.22.2 icmp_seq=2 ttl=61 time=10.828 ms
84 bytes from 10.13.22.2 icmp_seq=3 ttl=61 time=10.124 ms
84 bytes from 10.13.22.2 icmp_seq=4 ttl=61 time=11.429 ms
^C
pc_1> ping 10.13.33.2

84 bytes from 10.13.33.2 icmp_seq=1 ttl=61 time=8.277 ms
84 bytes from 10.13.33.2 icmp_seq=2 ttl=61 time=8.044 ms
84 bytes from 10.13.33.2 icmp_seq=3 ttl=61 time=18.607 ms
84 bytes from 10.13.33.2 icmp_seq=4 ttl=61 time=8.485 ms
^C
pc_1> ping 10.13.34.2

84 bytes from 10.13.34.2 icmp_seq=1 ttl=61 time=13.618 ms
84 bytes from 10.13.34.2 icmp_seq=2 ttl=61 time=17.369 ms
84 bytes from 10.13.34.2 icmp_seq=3 ttl=61 time=9.016 ms
84 bytes from 10.13.34.2 icmp_seq=4 ttl=61 time=9.983 ms
^C
pc_1>
```
</details>

<details>
<summary> ping с loopback0 LEAF_1 до остальных loopbacks LEAVES </summary>

```
Leaf_1# ping 10.10.1.0 source 10.10.10.0
PING 10.10.1.0 (10.10.1.0) from 10.10.10.0: 56 data bytes
64 bytes from 10.10.1.0: icmp_seq=0 ttl=254 time=5.449 ms
64 bytes from 10.10.1.0: icmp_seq=1 ttl=254 time=4.045 ms
64 bytes from 10.10.1.0: icmp_seq=2 ttl=254 time=3.713 ms
64 bytes from 10.10.1.0: icmp_seq=3 ttl=254 time=3.744 ms
64 bytes from 10.10.1.0: icmp_seq=4 ttl=254 time=3.894 ms

--- 10.10.1.0 ping statistics ---
5 packets transmitted, 5 packets received, 0.00% packet loss
round-trip min/avg/max = 3.713/4.169/5.449 ms
Leaf_1# ping 10.10.2.0 source 10.10.10.0
PING 10.10.2.0 (10.10.2.0) from 10.10.10.0: 56 data bytes
64 bytes from 10.10.2.0: icmp_seq=0 ttl=254 time=6.885 ms
64 bytes from 10.10.2.0: icmp_seq=1 ttl=254 time=3.774 ms
64 bytes from 10.10.2.0: icmp_seq=2 ttl=254 time=2.863 ms
64 bytes from 10.10.2.0: icmp_seq=3 ttl=254 time=2.934 ms
64 bytes from 10.10.2.0: icmp_seq=4 ttl=254 time=2.785 ms

--- 10.10.2.0 ping statistics ---
5 packets transmitted, 5 packets received, 0.00% packet loss
round-trip min/avg/max = 2.785/3.848/6.885 ms
Leaf_1# ping 10.10.20.0 source 10.10.10.0
PING 10.10.20.0 (10.10.20.0) from 10.10.10.0: 56 data bytes
64 bytes from 10.10.20.0: icmp_seq=0 ttl=253 time=5.561 ms
64 bytes from 10.10.20.0: icmp_seq=1 ttl=253 time=3.571 ms
64 bytes from 10.10.20.0: icmp_seq=2 ttl=253 time=4.388 ms
64 bytes from 10.10.20.0: icmp_seq=3 ttl=253 time=4.94 ms
64 bytes from 10.10.20.0: icmp_seq=4 ttl=253 time=4.376 ms

--- 10.10.20.0 ping statistics ---
5 packets transmitted, 5 packets received, 0.00% packet loss
round-trip min/avg/max = 3.571/4.567/5.561 ms
Leaf_1# ping 10.10.30.0 source 10.10.10.0
PING 10.10.30.0 (10.10.30.0) from 10.10.10.0: 56 data bytes
64 bytes from 10.10.30.0: icmp_seq=0 ttl=253 time=7.397 ms
64 bytes from 10.10.30.0: icmp_seq=1 ttl=253 time=4.264 ms
64 bytes from 10.10.30.0: icmp_seq=2 ttl=253 time=10.296 ms
64 bytes from 10.10.30.0: icmp_seq=3 ttl=253 time=6.523 ms
64 bytes from 10.10.30.0: icmp_seq=4 ttl=253 time=4.134 ms

--- 10.10.30.0 ping statistics ---
5 packets transmitted, 5 packets received, 0.00% packet loss
round-trip min/avg/max = 4.134/6.522/10.296 ms
Leaf_1#
```
</details>

# Заметки  
<details>
<summary> AES шифрование паролей в конфиге </summary>

```
Leaf_1(config)# feature password encryption aes

Leaf_1# key config-key ascii bgpbgpbgpbgpbgpbgp

Leaf_1# encryption re-encrypt obfuscated

Leaf_1# show keystore
Software Keystore
Type: P - PAC, S - Secret
Index           Type              Name
---------------------------------------
0               S                mkeyA

Leaf_1# show encryption mkey info all
Master-Key : 1
-------------------------------- --------------------------------------------------
    Type                        :       Running & Startup (Active)
    Key-Hash(first 16 chars)    :       SHA512: QQ2rcICJtFMhDjT4
    Protection-Type             :       Software
    Length                      :       18
    Last updated                :       2026-05-22 09:28:29  UTC
-------------------------------- --------------------------------------------------

Leaf_1# show encryption service status
Encryption service enabled
Master Encryption Key configured
Type-6 encryption is  being used
```
</details>

<details>
<summary> защита BGP без сертификатов </summary>

```
password - TCP MD5 Signature Option
ao - TCP Authentication Option (набор алгоритмов на выбор)
ao приоритетнее, чем password, когда введены обе команды, по понятным причинам 
```
</details>
