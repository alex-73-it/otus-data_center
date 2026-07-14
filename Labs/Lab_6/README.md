# Адресное пространство и схема сети
![](<Схема сети.png>)
# Конфигурации устройств
<details>
<summary> Конфиг Leaf_1 </summary>

```
nv overlay evpn
feature fabric forwarding
feature vn-segment-vlan-based
feature nv overlay

hardware access-list tcam region racl 512
hardware access-list tcam region arp-ether 256 double-wide

fabric forwarding anycast-gateway-mac 0000.0000.0001

vlan 1,11,100
vlan 11
  vn-segment 10011
vlan 100
  vn-segment 10100

interface Vlan11
  no shutdown
  vrf member l3vni
  ip address 10.13.11.1/24
  fabric forwarding mode anycast-gateway

interface Vlan100
  no shutdown
  vrf member l3vni
  ip forward

interface nve1
  no shutdown
  host-reachability protocol bgp
  source-interface loopback1
  global suppress-arp
  global ingress-replication protocol bgp
  member vni 10011
  member vni 10100 associate-vrf

key chain bgp tcp
  key 0
    send-id 0
    recv-id 0
    key-string 6 JDYkUZE1jWKy8FMY4B+jk2hcDyS+ePC60LCN3/ecIHQLFgNZJNSRMJcaXSOomRHhPiDF2Gyj1qKvAA==
    send-lifetime local 00:00:00 Jan 01 2026  infinite
    cryptographic-algorithm HMAC-SHA-256

router bgp 65011
  router-id 10.10.10.0
  timers bgp 3 9
  bestpath as-path multipath-relax
  reconnect-interval 10
  log-neighbor-changes
  address-family l2vpn evpn
    maximum-paths 2
  template peer TEMPL_EVPN_SPINES
    remote-as 65000
    ao bgp
    update-source loopback0
    ebgp-multihop 2
    timers 3 9
    address-family l2vpn evpn
      send-community
      send-community extended
      rewrite-evpn-rt-asn
  neighbor 10.10.1.0
    inherit peer TEMPL_EVPN_SPINES
  neighbor 10.10.2.0
    inherit peer TEMPL_EVPN_SPINES
evpn
  vni 10011 l2
    rd auto
    route-target import auto
    route-target export auto
vrf context l3vni
  vni 10100
  rd auto
  address-family ipv4 unicast
    route-target both auto
    route-target both auto evpn
```
</details>

<details>
<summary> Конфиг Leaf_2 </summary>

```
nv overlay evpn
feature fabric forwarding
feature vn-segment-vlan-based
feature nv overlay

hardware access-list tcam region racl 512
hardware access-list tcam region arp-ether 256 double-wide

fabric forwarding anycast-gateway-mac 0000.0000.0001

vlan 1,22,100
vlan 22
  vn-segment 10022
vlan 100
  vn-segment 10100

interface Vlan22
  no shutdown
  vrf member l3vni
  ip address 10.13.22.1/24
  fabric forwarding mode anycast-gateway

interface Vlan100
  no shutdown
  vrf member l3vni
  ip forward

interface nve1
  no shutdown
  host-reachability protocol bgp
  source-interface loopback1
  global suppress-arp
  global ingress-replication protocol bgp
  member vni 10022
  member vni 10100 associate-vrf

key chain bgp tcp
  key 0
    send-id 0
    recv-id 0
    key-string 6 JDYkfDvqbqQcb5S8AO4qwdFeSCQxcoDOfVOSqV0ExcsxkT93JLOzrrrCftyuEr4bx89g+hHQ32LMAA==
    send-lifetime local 00:00:00 Jan 01 2026  infinite
    cryptographic-algorithm HMAC-SHA-256

router bgp 65012
  router-id 10.10.20.0
  timers bgp 3 9
  bestpath as-path multipath-relax
  reconnect-interval 10
  log-neighbor-changes
  address-family l2vpn evpn
    maximum-paths 2
  template peer TEMPL_EVPN_SPINES
    remote-as 65000
    ao bgp
    update-source loopback0
    ebgp-multihop 2
    timers 3 9
    address-family l2vpn evpn
      send-community
      send-community extended
      rewrite-evpn-rt-asn
  neighbor 10.10.1.0
    inherit peer TEMPL_EVPN_SPINES
  neighbor 10.10.2.0
    inherit peer TEMPL_EVPN_SPINES
evpn
  vni 10022 l2
    rd auto
    route-target import auto
    route-target export auto
vrf context l3vni
  rd auto
  address-family ipv4 unicast
    route-target both auto
    route-target both auto evpn
```
</details>

<details>
<summary> Конфиг Leaf_3 </summary>

```
nv overlay evpn
feature fabric forwarding
feature vn-segment-vlan-based
feature nv overlay

hardware access-list tcam region racl 512
hardware access-list tcam region arp-ether 256 double-wide

fabric forwarding anycast-gateway-mac 0000.0000.0001

vlan 1,33-34,100
vlan 33
  vn-segment 10033
vlan 34
  vn-segment 10034
vlan 100
  vn-segment 10100

interface Vlan33
  no shutdown
  vrf member l3vni
  ip address 10.13.33.1/24
  fabric forwarding mode anycast-gateway

interface Vlan34
  no shutdown
  vrf member l3vni
  ip address 10.13.34.1/24
  fabric forwarding mode anycast-gateway

interface Vlan100
  no shutdown
  vrf member l3vni
  ip forward

interface nve1
  no shutdown
  host-reachability protocol bgp
  source-interface loopback1
  global suppress-arp
  global ingress-replication protocol bgp
  member vni 10033
  member vni 10034
  member vni 10100 associate-vrf

key chain bgp tcp
  key 0
    send-id 0
    recv-id 0
    key-string 6 JDYkY6o0eGxYwx9zW+hRMvZOxSQG8ovFfKuW5BNzcvq86ZXzJNR6uQSVNhTUhGmTITs+JtGcClo9AA==
    send-lifetime local 00:00:00 Jan 01 2026  infinite
    cryptographic-algorithm HMAC-SHA-256

router bgp 65013
  router-id 10.10.30.0
  timers bgp 3 9
  bestpath as-path multipath-relax
  reconnect-interval 10
  log-neighbor-changes
  address-family l2vpn evpn
    maximum-paths 2
  template peer TEMPL_EVPN_SPINES
    remote-as 65000
    ao bgp
    update-source loopback0
    ebgp-multihop 2
    timers 3 9
    address-family l2vpn evpn
      send-community
      send-community extended
      rewrite-evpn-rt-asn
  neighbor 10.10.1.0
    inherit peer TEMPL_EVPN_SPINES
  neighbor 10.10.2.0
    inherit peer TEMPL_EVPN_SPINES
evpn
  vni 10033 l2
    rd auto
    route-target import auto
    route-target export auto
  vni 10034 l2
    rd auto
    route-target import auto
    route-target export auto
vrf context l3vni
  rd auto
  address-family ipv4 unicast
    route-target both auto
    route-target both auto evpn
```
</details>

<details>
<summary> Конфиг Spine_1 </summary>

```
nv overlay evpn

route-map RM_NH_UNCHANGED permit 10
  set ip next-hop unchanged

key chain bgp tcp
  key 0
    send-id 0
    recv-id 0
    key-string 7 070d265c
    send-lifetime local 00:00:00 Jan 01 2026  infinite
    cryptographic-algorithm HMAC-SHA-256

router bgp 65000
  router-id 10.10.1.0
  timers bgp 3 9
  bestpath as-path multipath-relax
  reconnect-interval 10
  log-neighbor-changes
  address-family l2vpn evpn
    maximum-paths 2
    retain route-target all
  template peer TEMPL_EVPN_LEAVES
    ao bgp
    update-source loopback0
    ebgp-multihop 2
    timers 3 9
    address-family l2vpn evpn
      send-community
      send-community extended
      route-map RM_NH_UNCHANGED out
      rewrite-evpn-rt-asn
  neighbor 10.10.0.0/16 remote-as route-map RM_AS_LEAVES
    inherit peer TEMPL_EVPN_LEAVES
```
</details>

<details>
<summary> Конфиг Spine_2 </summary>

```
nv overlay evpn

route-map RM_NH_UNCHANGED permit 10
  set ip next-hop unchanged

key chain bgp tcp
  key 0
    send-id 0
    recv-id 0
    key-string 6 JDYkAb4tdsF6ksMsngRlPOKY0yQ7AAsSyboQiKZqQfg5RJk5JPhACUrWYBqZDJfHURu+EiI4L6PRAA==
    send-lifetime local 00:00:00 Jan 01 2026  infinite
    cryptographic-algorithm HMAC-SHA-256

router bgp 65000
  router-id 10.10.2.0
  timers bgp 3 9
  bestpath as-path multipath-relax
  reconnect-interval 10
  log-neighbor-changes
  address-family l2vpn evpn
    maximum-paths 2
    retain route-target all
  template peer TEMPL_EVPN_LEAVES
    ao bgp
    update-source loopback0
    ebgp-multihop 2
    timers 3 9
    address-family l2vpn evpn
      send-community
      send-community extended
      route-map RM_NH_UNCHANGED out
      rewrite-evpn-rt-asn
  neighbor 10.10.0.0/16 remote-as route-map RM_AS_LEAVES
    inherit peer TEMPL_EVPN_LEAVES
```
</details>

# Проверка работоспособности
1. Вывод команд show на устройствах
<details> 
<summary> Leaf_1 </summary>

```
Leaf_1# show mac address-table
Legend:
        * - primary entry, G - Gateway MAC, (R) - Routed MAC, O - Overlay MAC
        age - seconds since last seen,+ - primary entry using vPC Peer-Link,
        (T) - True, (F) - False, C - ControlPlane MAC, ~ - vsan,
        (NA)- Not Applicable A - ESI Active Path, S - ESI Standby Path
        TL - True Learned, PS - Peer Sync, RO - Re-originate
   VLAN     MAC Address      Type      age     Secure NTFY Ports
---------+-----------------+--------+---------+------+----+------------------
*   11     5000.5a00.800b   dynamic  NA         F      F    Eth1/3
*  100     5000.0100.1b08   static   -         F      F    Vlan100
*  100     5000.0200.1b08   static   -         F      F    nve1(10.11.20.0)
*  100     5000.0300.1b08   static   -         F      F    nve1(10.11.30.0)
G    -     0000.0000.0001   static   -         F      F    sup-eth1(R)
G    -     5000.0100.1b08   static   -         F      F    sup-eth1(R)
G   11     5000.0100.1b08   static   -         F      F    sup-eth1(R)
G  100     5000.0100.1b08   static   -         F      F    sup-eth1(R)
Leaf_1# show l2route mac all

Flags -(Rmac):Router MAC (Stt):Static (L):Local (R):Remote
(Dup):Duplicate (Spl):Split (Rcv):Recv (AD):Auto-Delete (D):Del Pending
(S):Stale (C):Clear, (Ps):Peer Sync (Ro):Re-Originated (Nho):NH-Override
(Asy):Asymmetric (Gw):Gateway
(Bh):Blackhole, (Dum):Dummy
(Pf):Permanently-Frozen, (Orp): Orphan
(PipOrp): Directly connected Orphan to PIP based vPC BGW
(PipPeerOrp): Orphan connected to peer of PIP based vPC BGW

NH Flags- (Asy): Asymmetric VNI (RS): Remote Site Flag
          (GU): Group Policy Unaware

Topology    Mac Address    Prod   Flags              Seq No     Next-Hops
----------- -------------- ------ ------------------ ---------- ---------------------------------------------------------
11          5000.5a00.800b Local  L,                 0          Eth1/3
100         5000.0200.1b08 VXLAN  Rmac,              0          10.11.20.0
100         5000.0300.1b08 VXLAN  Rmac,              0          10.11.30.0
Leaf_1# show nve internal bgp rnh database
--------------------------------------------
Total peer-vni msgs recvd from bgp: 4
Peer add requests: 3
Peer update requests: 0
Peer delete requests: 1
Peer add/update requests: 3
Peer add ignored (peer exists): 0
Peer update ignored (invalid opc): 0
Peer delete ignored (invalid opc): 0
Peer add/update ignored (malloc error): 0
Peer add/update ignored (vni not cp): 0
Peer delete ignored (vni not cp): 0
--------------------------------------------
Showing BGP RNH Database, size : 2 vni 0

Flag codes: 0 - ISSU Done/ISSU N/A        1 - ADD_ISSU_PENDING
            2 - DEL_ISSU_PENDING          3 - UPD_ISSU_PENDING


VNI       Peer-IP            Peer-MAC            Tunnel-ID  Encap     (A /S ) Flags PT    IRB   Egress-VNI Underlay-VRF TE-Flags     GPC
10100     10.11.20.0         5000.0200.1b08      0xa0b1400  vxlan     (1 /0 ) 0     FAB   SYM   10100      1            0            0
10100     10.11.30.0         5000.0300.1b08      0xa0b1e00  vxlan     (1 /0 ) 0     FAB   SYM   10100      1            0            0

Leaf_1# show bgp l2vpn evpn
BGP routing table information for VRF default, address family L2VPN EVPN
BGP table version is 112, Local Router ID is 10.10.10.0
Status: s-suppressed, x-deleted, S-stale, d-dampened, h-history, *-valid, >-best
Path type: i-internal, e-external, c-confed, l-local, a-aggregate, r-redist, I-injected
Origin codes: i - IGP, e - EGP, ? - incomplete, | - multipath, & - backup, 2 - best2

   Network            Next Hop            Metric     LocPrf     Weight Path
Route Distinguisher: 10.10.10.0:32778    (L2VNI 10011)
*>l[2]:[0]:[0]:[48]:[5000.5a00.800b]:[0]:[0.0.0.0]/216
                      10.11.10.0                        100      32768 i
*>l[2]:[0]:[0]:[48]:[5000.5a00.800b]:[32]:[10.13.11.2]/272
                      10.11.10.0                        100      32768 i
*>l[3]:[0]:[32]:[10.11.10.0]/88
                      10.11.10.0                        100      32768 i

Route Distinguisher: 10.10.20.0:32789
* e[2]:[0]:[0]:[48]:[505d.5100.8016]:[32]:[10.13.22.2]/272
                      10.11.20.0                                     0 65000 65012 i
*>e                   10.11.20.0                                     0 65000 65012 i

Route Distinguisher: 10.10.30.0:32800
*>e[2]:[0]:[0]:[48]:[50af.2100.8021]:[32]:[10.13.33.2]/272
                      10.11.30.0                                     0 65000 65013 i
* e                   10.11.30.0                                     0 65000 65013 i

Route Distinguisher: 10.10.30.0:32801
* e[2]:[0]:[0]:[48]:[50f2.9700.8022]:[32]:[10.13.34.2]/272
                      10.11.30.0                                     0 65000 65013 i
*>e                   10.11.30.0                                     0 65000 65013 i

Route Distinguisher: 10.10.10.0:4    (L3VNI 10100)
*>e[2]:[0]:[0]:[48]:[505d.5100.8016]:[32]:[10.13.22.2]/272
                      10.11.20.0                                     0 65000 65012 i
*>e[2]:[0]:[0]:[48]:[50af.2100.8021]:[32]:[10.13.33.2]/272
                      10.11.30.0                                     0 65000 65013 i
*>e[2]:[0]:[0]:[48]:[50f2.9700.8022]:[32]:[10.13.34.2]/272
                      10.11.30.0                                     0 65000 65013 i

Leaf_1# show bgp l3vpn
VRF-Name                  VRF-ID RD       State   Reason
l3vni                          4 10.10.10.0:4 UP      --
Leaf_1# show ip route vrf l3vni
IP Route Table for VRF "l3vni"
'*' denotes best ucast next-hop
'**' denotes best mcast next-hop
'[x/y]' denotes [preference/metric]
'%<string>' in via output denotes VRF <string>

10.13.11.0/24, ubest/mbest: 1/0, attached
    *via 10.13.11.1, Vlan11, [0/0], 1d01h, direct
10.13.11.1/32, ubest/mbest: 1/0, attached
    *via 10.13.11.1, Vlan11, [0/0], 1d01h, local
10.13.11.2/32, ubest/mbest: 1/0, attached
    *via 10.13.11.2, Vlan11, [190/0], 00:51:58, hmm
10.13.22.2/32, ubest/mbest: 1/0
    *via 10.11.20.0%default, [20/0], 1d00h, bgp-65011, external, tag 65000, segid: 10100 tunnelid: 0xa0b1400 encap: VXLAN
10.13.33.2/32, ubest/mbest: 1/0
    *via 10.11.30.0%default, [20/0], 23:41:43, bgp-65011, external, tag 65000, segid: 10100 tunnelid: 0xa0b1e00 encap: VXLAN
10.13.34.2/32, ubest/mbest: 1/0
    *via 10.11.30.0%default, [20/0], 23:41:13, bgp-65011, external, tag 65000, segid: 10100 tunnelid: 0xa0b1e00 encap: VXLAN

Leaf_1#
```
</details>

<details> 
<summary> Leaf_2 </summary>

```
Leaf_2# show mac address-table
Legend:
        * - primary entry, G - Gateway MAC, (R) - Routed MAC, O - Overlay MAC
        age - seconds since last seen,+ - primary entry using vPC Peer-Link,
        (T) - True, (F) - False, C - ControlPlane MAC, ~ - vsan,
        (NA)- Not Applicable A - ESI Active Path, S - ESI Standby Path
        TL - True Learned, PS - Peer Sync, RO - Re-originate
   VLAN     MAC Address      Type      age     Secure NTFY Ports
---------+-----------------+--------+---------+------+----+------------------
*   22     505d.5100.8016   dynamic  NA         F      F    Eth1/3
*  100     5000.0100.1b08   static   -         F      F    nve1(10.11.10.0)
*  100     5000.0200.1b08   static   -         F      F    Vlan100
*  100     5000.0300.1b08   static   -         F      F    nve1(10.11.30.0)
G    -     0000.0000.0001   static   -         F      F    sup-eth1(R)
G    -     5000.0200.1b08   static   -         F      F    sup-eth1(R)
G   22     5000.0200.1b08   static   -         F      F    sup-eth1(R)
G  100     5000.0200.1b08   static   -         F      F    sup-eth1(R)
Leaf_2# show l2route mac all

Flags -(Rmac):Router MAC (Stt):Static (L):Local (R):Remote
(Dup):Duplicate (Spl):Split (Rcv):Recv (AD):Auto-Delete (D):Del Pending
(S):Stale (C):Clear, (Ps):Peer Sync (Ro):Re-Originated (Nho):NH-Override
(Asy):Asymmetric (Gw):Gateway
(Bh):Blackhole, (Dum):Dummy
(Pf):Permanently-Frozen, (Orp): Orphan
(PipOrp): Directly connected Orphan to PIP based vPC BGW
(PipPeerOrp): Orphan connected to peer of PIP based vPC BGW

NH Flags- (Asy): Asymmetric VNI (RS): Remote Site Flag
          (GU): Group Policy Unaware

Topology    Mac Address    Prod   Flags              Seq No     Next-Hops
----------- -------------- ------ ------------------ ---------- ---------------------------------------------------------
22          505d.5100.8016 Local  L,                 0          Eth1/3
100         5000.0100.1b08 VXLAN  Rmac,              0          10.11.10.0
100         5000.0300.1b08 VXLAN  Rmac,              0          10.11.30.0
Leaf_2# show nve internal bgp rnh database
--------------------------------------------
Total peer-vni msgs recvd from bgp: 6
Peer add requests: 4
Peer update requests: 0
Peer delete requests: 2
Peer add/update requests: 4
Peer add ignored (peer exists): 0
Peer update ignored (invalid opc): 0
Peer delete ignored (invalid opc): 0
Peer add/update ignored (malloc error): 0
Peer add/update ignored (vni not cp): 0
Peer delete ignored (vni not cp): 0
--------------------------------------------
Showing BGP RNH Database, size : 2 vni 0

Flag codes: 0 - ISSU Done/ISSU N/A        1 - ADD_ISSU_PENDING
            2 - DEL_ISSU_PENDING          3 - UPD_ISSU_PENDING


VNI       Peer-IP            Peer-MAC            Tunnel-ID  Encap     (A /S ) Flags PT    IRB   Egress-VNI Underlay-VRF TE-Flags     GPC
10100     10.11.10.0         5000.0100.1b08      0xa0b0a00  vxlan     (1 /0 ) 0     FAB   SYM   10100      1            0            0
10100     10.11.30.0         5000.0300.1b08      0xa0b1e00  vxlan     (1 /0 ) 0     FAB   SYM   10100      1            0            0

Leaf_2# show bgp l2vpn evpn
BGP routing table information for VRF default, address family L2VPN EVPN
BGP table version is 102, Local Router ID is 10.10.20.0
Status: s-suppressed, x-deleted, S-stale, d-dampened, h-history, *-valid, >-best
Path type: i-internal, e-external, c-confed, l-local, a-aggregate, r-redist, I-injected
Origin codes: i - IGP, e - EGP, ? - incomplete, | - multipath, & - backup, 2 - best2

   Network            Next Hop            Metric     LocPrf     Weight Path
Route Distinguisher: 10.10.10.0:32778
* e[2]:[0]:[0]:[48]:[5000.5a00.800b]:[32]:[10.13.11.2]/272
                      10.11.10.0                                     0 65000 65011 i
*>e                   10.11.10.0                                     0 65000 65011 i

Route Distinguisher: 10.10.20.0:32789    (L2VNI 10022)
*>l[2]:[0]:[0]:[48]:[505d.5100.8016]:[0]:[0.0.0.0]/216
                      10.11.20.0                        100      32768 i
*>l[2]:[0]:[0]:[48]:[505d.5100.8016]:[32]:[10.13.22.2]/272
                      10.11.20.0                        100      32768 i
*>l[3]:[0]:[32]:[10.11.20.0]/88
                      10.11.20.0                        100      32768 i

Route Distinguisher: 10.10.30.0:32800
*>e[2]:[0]:[0]:[48]:[50af.2100.8021]:[32]:[10.13.33.2]/272
                      10.11.30.0                                     0 65000 65013 i
* e                   10.11.30.0                                     0 65000 65013 i

Route Distinguisher: 10.10.30.0:32801
*>e[2]:[0]:[0]:[48]:[50f2.9700.8022]:[32]:[10.13.34.2]/272
                      10.11.30.0                                     0 65000 65013 i
* e                   10.11.30.0                                     0 65000 65013 i

Route Distinguisher: 10.10.20.0:4    (L3VNI 10100)
*>e[2]:[0]:[0]:[48]:[5000.5a00.800b]:[32]:[10.13.11.2]/272
                      10.11.10.0                                     0 65000 65011 i
*>e[2]:[0]:[0]:[48]:[50af.2100.8021]:[32]:[10.13.33.2]/272
                      10.11.30.0                                     0 65000 65013 i
*>e[2]:[0]:[0]:[48]:[50f2.9700.8022]:[32]:[10.13.34.2]/272
                      10.11.30.0                                     0 65000 65013 i

Leaf_2# show bgp l3vpn
VRF-Name                  VRF-ID RD       State   Reason
l3vni                          4 10.10.20.0:4 UP      --
Leaf_2# show ip
ip     ipv6
Leaf_2# show ip route vrf l3vni
IP Route Table for VRF "l3vni"
'*' denotes best ucast next-hop
'**' denotes best mcast next-hop
'[x/y]' denotes [preference/metric]
'%<string>' in via output denotes VRF <string>

10.13.11.2/32, ubest/mbest: 1/0
    *via 10.11.10.0%default, [20/0], 1d00h, bgp-65012, external, tag 65000, segid: 10100 tunnelid: 0xa0b0a00 encap: VXLAN
10.13.22.0/24, ubest/mbest: 1/0, attached
    *via 10.13.22.1, Vlan22, [0/0], 1d01h, direct
10.13.22.1/32, ubest/mbest: 1/0, attached
    *via 10.13.22.1, Vlan22, [0/0], 1d01h, local
10.13.22.2/32, ubest/mbest: 1/0, attached
    *via 10.13.22.2, Vlan22, [190/0], 00:16:13, hmm
10.13.33.2/32, ubest/mbest: 1/0
    *via 10.11.30.0%default, [20/0], 23:43:12, bgp-65012, external, tag 65000, segid: 10100 tunnelid: 0xa0b1e00 encap: VXLAN
10.13.34.2/32, ubest/mbest: 1/0
    *via 10.11.30.0%default, [20/0], 23:42:42, bgp-65012, external, tag 65000, segid: 10100 tunnelid: 0xa0b1e00 encap: VXLAN

Leaf_2#
```
</details>

<details> 
<summary> Leaf_3 </summary>

```
Leaf_3# show mac address-table
Legend:
        * - primary entry, G - Gateway MAC, (R) - Routed MAC, O - Overlay MAC
        age - seconds since last seen,+ - primary entry using vPC Peer-Link,
        (T) - True, (F) - False, C - ControlPlane MAC, ~ - vsan,
        (NA)- Not Applicable A - ESI Active Path, S - ESI Standby Path
        TL - True Learned, PS - Peer Sync, RO - Re-originate
   VLAN     MAC Address      Type      age     Secure NTFY Ports
---------+-----------------+--------+---------+------+----+------------------
*   33     50af.2100.8021   dynamic  NA         F      F    Eth1/3
*   34     50f2.9700.8022   dynamic  NA         F      F    Eth1/4
*  100     5000.0100.1b08   static   -         F      F    nve1(10.11.10.0)
*  100     5000.0200.1b08   static   -         F      F    nve1(10.11.20.0)
*  100     5000.0300.1b08   static   -         F      F    Vlan100
G    -     0000.0000.0001   static   -         F      F    sup-eth1(R)
G    -     5000.0300.1b08   static   -         F      F    sup-eth1(R)
G   33     5000.0300.1b08   static   -         F      F    sup-eth1(R)
G   34     5000.0300.1b08   static   -         F      F    sup-eth1(R)
G  100     5000.0300.1b08   static   -         F      F    sup-eth1(R)
Leaf_3# show l2route mac all

Flags -(Rmac):Router MAC (Stt):Static (L):Local (R):Remote
(Dup):Duplicate (Spl):Split (Rcv):Recv (AD):Auto-Delete (D):Del Pending
(S):Stale (C):Clear, (Ps):Peer Sync (Ro):Re-Originated (Nho):NH-Override
(Asy):Asymmetric (Gw):Gateway
(Bh):Blackhole, (Dum):Dummy
(Pf):Permanently-Frozen, (Orp): Orphan
(PipOrp): Directly connected Orphan to PIP based vPC BGW
(PipPeerOrp): Orphan connected to peer of PIP based vPC BGW

NH Flags- (Asy): Asymmetric VNI (RS): Remote Site Flag
          (GU): Group Policy Unaware

Topology    Mac Address    Prod   Flags              Seq No     Next-Hops
----------- -------------- ------ ------------------ ---------- ---------------------------------------------------------
33          50af.2100.8021 Local  L,                 0          Eth1/3
34          50f2.9700.8022 Local  L,                 0          Eth1/4
100         5000.0100.1b08 VXLAN  Rmac,              0          10.11.10.0
100         5000.0200.1b08 VXLAN  Rmac,              0          10.11.20.0
Leaf_3# show nve internal bgp rnh database
--------------------------------------------
Total peer-vni msgs recvd from bgp: 3
Peer add requests: 4
Peer update requests: 0
Peer delete requests: 2
Peer add/update requests: 4
Peer add ignored (peer exists): 0
Peer update ignored (invalid opc): 0
Peer delete ignored (invalid opc): 0
Peer add/update ignored (malloc error): 0
Peer add/update ignored (vni not cp): 0
Peer delete ignored (vni not cp): 0
--------------------------------------------
Showing BGP RNH Database, size : 2 vni 0

Flag codes: 0 - ISSU Done/ISSU N/A        1 - ADD_ISSU_PENDING
            2 - DEL_ISSU_PENDING          3 - UPD_ISSU_PENDING


VNI       Peer-IP            Peer-MAC            Tunnel-ID  Encap     (A /S ) Flags PT    IRB   Egress-VNI Underlay-VRF TE-Flags     GPC
10100     10.11.10.0         5000.0100.1b08      0xa0b0a00  vxlan     (1 /0 ) 0     FAB   SYM   10100      1            0            0
10100     10.11.20.0         5000.0200.1b08      0xa0b1400  vxlan     (1 /0 ) 0     FAB   SYM   10100      1            0            0

Leaf_3# show bgp l2vpn evpn
BGP routing table information for VRF default, address family L2VPN EVPN
BGP table version is 194, Local Router ID is 10.10.30.0
Status: s-suppressed, x-deleted, S-stale, d-dampened, h-history, *-valid, >-best
Path type: i-internal, e-external, c-confed, l-local, a-aggregate, r-redist, I-injected
Origin codes: i - IGP, e - EGP, ? - incomplete, | - multipath, & - backup, 2 - best2

   Network            Next Hop            Metric     LocPrf     Weight Path
Route Distinguisher: 10.10.10.0:32778
* e[2]:[0]:[0]:[48]:[5000.5a00.800b]:[32]:[10.13.11.2]/272
                      10.11.10.0                                     0 65000 65011 i
*>e                   10.11.10.0                                     0 65000 65011 i

Route Distinguisher: 10.10.20.0:32789
* e[2]:[0]:[0]:[48]:[505d.5100.8016]:[32]:[10.13.22.2]/272
                      10.11.20.0                                     0 65000 65012 i
*>e                   10.11.20.0                                     0 65000 65012 i

Route Distinguisher: 10.10.30.0:32800    (L2VNI 10033)
*>l[2]:[0]:[0]:[48]:[50af.2100.8021]:[0]:[0.0.0.0]/216
                      10.11.30.0                        100      32768 i
*>l[2]:[0]:[0]:[48]:[50af.2100.8021]:[32]:[10.13.33.2]/272
                      10.11.30.0                        100      32768 i
*>l[3]:[0]:[32]:[10.11.30.0]/88
                      10.11.30.0                        100      32768 i

Route Distinguisher: 10.10.30.0:32801    (L2VNI 10034)
*>l[2]:[0]:[0]:[48]:[50f2.9700.8022]:[0]:[0.0.0.0]/216
                      10.11.30.0                        100      32768 i
*>l[2]:[0]:[0]:[48]:[50f2.9700.8022]:[32]:[10.13.34.2]/272
                      10.11.30.0                        100      32768 i
*>l[3]:[0]:[32]:[10.11.30.0]/88
                      10.11.30.0                        100      32768 i

Route Distinguisher: 10.10.30.0:4    (L3VNI 10100)
*>e[2]:[0]:[0]:[48]:[5000.5a00.800b]:[32]:[10.13.11.2]/272
                      10.11.10.0                                     0 65000 65011 i
*>e[2]:[0]:[0]:[48]:[505d.5100.8016]:[32]:[10.13.22.2]/272
                      10.11.20.0                                     0 65000 65012 i

Leaf_3# show bgp l3vpn
VRF-Name                  VRF-ID RD       State   Reason
l3vni                          4 10.10.30.0:4 UP      --
Leaf_3# show ip route vrf l3vni
IP Route Table for VRF "l3vni"
'*' denotes best ucast next-hop
'**' denotes best mcast next-hop
'[x/y]' denotes [preference/metric]
'%<string>' in via output denotes VRF <string>

10.13.11.2/32, ubest/mbest: 1/0
    *via 10.11.10.0%default, [20/0], 23:45:32, bgp-65013, external, tag 65000, segid: 10100 tunnelid: 0xa0b0a00 encap: VXLAN
10.13.22.2/32, ubest/mbest: 1/0
    *via 10.11.20.0%default, [20/0], 23:45:32, bgp-65013, external, tag 65000, segid: 10100 tunnelid: 0xa0b1400 encap: VXLAN
10.13.33.0/24, ubest/mbest: 1/0, attached
    *via 10.13.33.1, Vlan33, [0/0], 23:51:46, direct
10.13.33.1/32, ubest/mbest: 1/0, attached
    *via 10.13.33.1, Vlan33, [0/0], 23:51:46, local
10.13.33.2/32, ubest/mbest: 1/0, attached
    *via 10.13.33.2, Vlan33, [190/0], 00:45:39, hmm
10.13.34.0/24, ubest/mbest: 1/0, attached
    *via 10.13.34.1, Vlan34, [0/0], 23:51:11, direct
10.13.34.1/32, ubest/mbest: 1/0, attached
    *via 10.13.34.1, Vlan34, [0/0], 23:51:11, local
10.13.34.2/32, ubest/mbest: 1/0, attached
    *via 10.13.34.2, Vlan34, [190/0], 00:45:39, hmm

Leaf_3#
```
</details>

<details> 
<summary> Spine_1 </summary>

```
Spine_1# show bgp l2vpn evpn
BGP routing table information for VRF default, address family L2VPN EVPN
BGP table version is 464, Local Router ID is 10.10.1.0
Status: s-suppressed, x-deleted, S-stale, d-dampened, h-history, *-valid, >-best
Path type: i-internal, e-external, c-confed, l-local, a-aggregate, r-redist, I-injected
Origin codes: i - IGP, e - EGP, ? - incomplete, | - multipath, & - backup, 2 - best2

   Network            Next Hop            Metric     LocPrf     Weight Path
Route Distinguisher: 10.10.10.0:32778
*>e[2]:[0]:[0]:[48]:[5000.5a00.800b]:[0]:[0.0.0.0]/216
                      10.11.10.0                                     0 65011 i
*>e[2]:[0]:[0]:[48]:[5000.5a00.800b]:[32]:[10.13.11.2]/272
                      10.11.10.0                                     0 65011 i
*>e[3]:[0]:[32]:[10.11.10.0]/88
                      10.11.10.0                                     0 65011 i

Route Distinguisher: 10.10.20.0:32789
*>e[2]:[0]:[0]:[48]:[505d.5100.8016]:[0]:[0.0.0.0]/216
                      10.11.20.0                                     0 65012 i
*>e[2]:[0]:[0]:[48]:[505d.5100.8016]:[32]:[10.13.22.2]/272
                      10.11.20.0                                     0 65012 i
*>e[3]:[0]:[32]:[10.11.20.0]/88
                      10.11.20.0                                     0 65012 i

Route Distinguisher: 10.10.30.0:32800
*>e[2]:[0]:[0]:[48]:[50af.2100.8021]:[0]:[0.0.0.0]/216
                      10.11.30.0                                     0 65013 i
*>e[2]:[0]:[0]:[48]:[50af.2100.8021]:[32]:[10.13.33.2]/272
                      10.11.30.0                                     0 65013 i
*>e[3]:[0]:[32]:[10.11.30.0]/88
                      10.11.30.0                                     0 65013 i

Route Distinguisher: 10.10.30.0:32801
*>e[2]:[0]:[0]:[48]:[50f2.9700.8022]:[0]:[0.0.0.0]/216
                      10.11.30.0                                     0 65013 i
*>e[2]:[0]:[0]:[48]:[50f2.9700.8022]:[32]:[10.13.34.2]/272
                      10.11.30.0                                     0 65013 i
*>e[3]:[0]:[32]:[10.11.30.0]/88
                      10.11.30.0                                     0 65013 i

Spine_1#
```
</details>

<details> 
<summary> Spine_2 </summary>

```
Spine_2# show bgp l2vpn evpn
BGP routing table information for VRF default, address family L2VPN EVPN
BGP table version is 337, Local Router ID is 10.10.2.0
Status: s-suppressed, x-deleted, S-stale, d-dampened, h-history, *-valid, >-best
Path type: i-internal, e-external, c-confed, l-local, a-aggregate, r-redist, I-injected
Origin codes: i - IGP, e - EGP, ? - incomplete, | - multipath, & - backup, 2 - best2

   Network            Next Hop            Metric     LocPrf     Weight Path
Route Distinguisher: 10.10.10.0:32778
*>e[2]:[0]:[0]:[48]:[5000.5a00.800b]:[0]:[0.0.0.0]/216
                      10.11.10.0                                     0 65011 i
*>e[2]:[0]:[0]:[48]:[5000.5a00.800b]:[32]:[10.13.11.2]/272
                      10.11.10.0                                     0 65011 i
*>e[3]:[0]:[32]:[10.11.10.0]/88
                      10.11.10.0                                     0 65011 i

Route Distinguisher: 10.10.20.0:32789
*>e[2]:[0]:[0]:[48]:[505d.5100.8016]:[0]:[0.0.0.0]/216
                      10.11.20.0                                     0 65012 i
*>e[2]:[0]:[0]:[48]:[505d.5100.8016]:[32]:[10.13.22.2]/272
                      10.11.20.0                                     0 65012 i
*>e[3]:[0]:[32]:[10.11.20.0]/88
                      10.11.20.0                                     0 65012 i

Route Distinguisher: 10.10.30.0:32800
*>e[2]:[0]:[0]:[48]:[50af.2100.8021]:[0]:[0.0.0.0]/216
                      10.11.30.0                                     0 65013 i
*>e[2]:[0]:[0]:[48]:[50af.2100.8021]:[32]:[10.13.33.2]/272
                      10.11.30.0                                     0 65013 i
*>e[3]:[0]:[32]:[10.11.30.0]/88
                      10.11.30.0                                     0 65013 i

Route Distinguisher: 10.10.30.0:32801
*>e[2]:[0]:[0]:[48]:[50f2.9700.8022]:[0]:[0.0.0.0]/216
                      10.11.30.0                                     0 65013 i
*>e[2]:[0]:[0]:[48]:[50f2.9700.8022]:[32]:[10.13.34.2]/272
                      10.11.30.0                                     0 65013 i
*>e[3]:[0]:[32]:[10.11.30.0]/88
                      10.11.30.0                                     0 65013 i

Spine_2#
```
</details>

2. Проверка доступности сетей c помощью ping
<details>
<summary> ping с Host_1 до осатльных Hosts </summary>

```
Host_1#ping 10.13.22.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.13.22.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 10/15/23 ms
Host_1#ping 10.13.33.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.13.33.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 12/14/17 ms
Host_1#ping 10.13.34.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.13.34.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 10/11/16 ms
Host_1#
```
</details>
