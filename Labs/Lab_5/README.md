# Адресное пространство и схема сети
![](<Схема сети.png>)
# Конфигурации устройств
<details>
<summary> Конфиг Leaf_1 </summary>

```
feature vn-segment-vlan-based
feature nv overlay
nv overlay evpn

vlan 11
  vn-segment 10011

interface nve1
  no shutdown
  host-reachability protocol bgp
  source-interface loopback1
  member vni 10011
    ingress-replication protocol bgp

evpn
  vni 10011 l2
    rd auto
    route-target import auto
    route-target export auto

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
```
</details>

<details>
<summary> Конфиг Leaf_2 </summary>

```
feature vn-segment-vlan-based
feature nv overlay
nv overlay evpn

vlan 11
  vn-segment 10011

interface nve1
  no shutdown
  host-reachability protocol bgp
  source-interface loopback1
  member vni 10011
    ingress-replication protocol bgp

evpn
  vni 10011 l2
    rd auto
    route-target import auto
    route-target export auto

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
```
</details>

<details>
<summary> Конфиг Leaf_3 </summary>

```
feature vn-segment-vlan-based
feature nv overlay
nv overlay evpn

vlan 11
  vn-segment 10011

interface nve1
  no shutdown
  host-reachability protocol bgp
  source-interface loopback1
  member vni 10011
    ingress-replication protocol bgp

evpn
  vni 10011 l2
    rd auto
    route-target import auto
    route-target export auto

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
C   11     505d.5100.800b   dynamic  NA         F      F    nve1(10.11.20.0)
C   11     50af.2100.800b   dynamic  NA         F      F    nve1(10.11.30.0)
C   11     50f2.9700.800b   dynamic  NA         F      F    nve1(10.11.30.0)
G    -     5000.0100.1b08   static   -         F      F    sup-eth1(R)
Leaf_1# show l2route mac topology 11

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
11          505d.5100.800b BGP    Rcv                0          10.11.20.0 (Label: 10011)
11          50af.2100.800b BGP    Rcv                0          10.11.30.0 (Label: 10011)
11          50f2.9700.800b BGP    Rcv                0          10.11.30.0 (Label: 10011)
Leaf_1# show nve internal bgp rnh database
--------------------------------------------
Total peer-vni msgs recvd from bgp: 2
Peer add requests: 2
Peer update requests: 0
Peer delete requests: 0
Peer add/update requests: 2
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
10011     10.11.20.0         0000.0000.0000      0x0        vxlan     (1 /0 ) 0     FAB   SYM   10011      1            0            0
10011     10.11.30.0         0000.0000.0000      0x0        vxlan     (1 /0 ) 0     FAB   SYM   10011      1            0            0

Leaf_1# show bgp l2vpn evpn
BGP routing table information for VRF default, address family L2VPN EVPN
BGP table version is 22, Local Router ID is 10.10.10.0
Status: s-suppressed, x-deleted, S-stale, d-dampened, h-history, *-valid, >-best
Path type: i-internal, e-external, c-confed, l-local, a-aggregate, r-redist, I-injected
Origin codes: i - IGP, e - EGP, ? - incomplete, | - multipath, & - backup, 2 - best2

   Network            Next Hop            Metric     LocPrf     Weight Path
Route Distinguisher: 10.10.10.0:32778    (L2VNI 10011)
*>l[2]:[0]:[0]:[48]:[5000.5a00.800b]:[0]:[0.0.0.0]/216
                      10.11.10.0                        100      32768 i
*>e[2]:[0]:[0]:[48]:[505d.5100.800b]:[0]:[0.0.0.0]/216
                      10.11.20.0                                     0 65000 65012 i
*>e[2]:[0]:[0]:[48]:[50af.2100.800b]:[0]:[0.0.0.0]/216
                      10.11.30.0                                     0 65000 65013 i
*>e[2]:[0]:[0]:[48]:[50f2.9700.800b]:[0]:[0.0.0.0]/216
                      10.11.30.0                                     0 65000 65013 i
*>l[3]:[0]:[32]:[10.11.10.0]/88
                      10.11.10.0                        100      32768 i
*>e[3]:[0]:[32]:[10.11.20.0]/88
                      10.11.20.0                                     0 65000 65012 i
*>e[3]:[0]:[32]:[10.11.30.0]/88
                      10.11.30.0                                     0 65000 65013 i

Route Distinguisher: 10.10.20.0:32778
* e[2]:[0]:[0]:[48]:[505d.5100.800b]:[0]:[0.0.0.0]/216
                      10.11.20.0                                     0 65000 65012 i
*>e                   10.11.20.0                                     0 65000 65012 i
* e[3]:[0]:[32]:[10.11.20.0]/88
                      10.11.20.0                                     0 65000 65012 i
*>e                   10.11.20.0                                     0 65000 65012 i

Route Distinguisher: 10.10.30.0:32778
* e[2]:[0]:[0]:[48]:[50af.2100.800b]:[0]:[0.0.0.0]/216
                      10.11.30.0                                     0 65000 65013 i
*>e                   10.11.30.0                                     0 65000 65013 i
* e[2]:[0]:[0]:[48]:[50f2.9700.800b]:[0]:[0.0.0.0]/216
                      10.11.30.0                                     0 65000 65013 i
*>e                   10.11.30.0                                     0 65000 65013 i
*>e[3]:[0]:[32]:[10.11.30.0]/88
                      10.11.30.0                                     0 65000 65013 i
* e                   10.11.30.0                                     0 65000 65013 i

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
C   11     5000.5a00.800b   dynamic  NA         F      F    nve1(10.11.10.0)
*   11     505d.5100.800b   dynamic  NA         F      F    Eth1/3
C   11     50af.2100.800b   dynamic  NA         F      F    nve1(10.11.30.0)
C   11     50f2.9700.800b   dynamic  NA         F      F    nve1(10.11.30.0)
G    -     5000.0200.1b08   static   -         F      F    sup-eth1(R)
Leaf_2# show l2route mac topology 11

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
11          5000.5a00.800b BGP    Rcv                0          10.11.10.0 (Label: 10011)
11          505d.5100.800b Local  L,                 0          Eth1/3
11          50af.2100.800b BGP    Rcv                0          10.11.30.0 (Label: 10011)
11          50f2.9700.800b BGP    Rcv                0          10.11.30.0 (Label: 10011)
Leaf_2# show nve internal bgp rnh database
--------------------------------------------
Total peer-vni msgs recvd from bgp: 1
Peer add requests: 2
Peer update requests: 0
Peer delete requests: 0
Peer add/update requests: 2
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
10011     10.11.10.0         0000.0000.0000      0x0        vxlan     (1 /0 ) 0     FAB   SYM   10011      1            0            0
10011     10.11.30.0         0000.0000.0000      0x0        vxlan     (1 /0 ) 0     FAB   SYM   10011      1            0            0

Leaf_2# show bgp l2vpn evpn
BGP routing table information for VRF default, address family L2VPN EVPN
BGP table version is 21, Local Router ID is 10.10.20.0
Status: s-suppressed, x-deleted, S-stale, d-dampened, h-history, *-valid, >-best
Path type: i-internal, e-external, c-confed, l-local, a-aggregate, r-redist, I-injected
Origin codes: i - IGP, e - EGP, ? - incomplete, | - multipath, & - backup, 2 - best2

   Network            Next Hop            Metric     LocPrf     Weight Path
Route Distinguisher: 10.10.10.0:32778
*>e[2]:[0]:[0]:[48]:[5000.5a00.800b]:[0]:[0.0.0.0]/216
                      10.11.10.0                                     0 65000 65011 i
* e                   10.11.10.0                                     0 65000 65011 i
* e[3]:[0]:[32]:[10.11.10.0]/88
                      10.11.10.0                                     0 65000 65011 i
*>e                   10.11.10.0                                     0 65000 65011 i

Route Distinguisher: 10.10.20.0:32778    (L2VNI 10011)
*>e[2]:[0]:[0]:[48]:[5000.5a00.800b]:[0]:[0.0.0.0]/216
                      10.11.10.0                                     0 65000 65011 i
*>l[2]:[0]:[0]:[48]:[505d.5100.800b]:[0]:[0.0.0.0]/216
                      10.11.20.0                        100      32768 i
*>e[2]:[0]:[0]:[48]:[50af.2100.800b]:[0]:[0.0.0.0]/216
                      10.11.30.0                                     0 65000 65013 i
*>e[2]:[0]:[0]:[48]:[50f2.9700.800b]:[0]:[0.0.0.0]/216
                      10.11.30.0                                     0 65000 65013 i
*>e[3]:[0]:[32]:[10.11.10.0]/88
                      10.11.10.0                                     0 65000 65011 i
*>l[3]:[0]:[32]:[10.11.20.0]/88
                      10.11.20.0                        100      32768 i
*>e[3]:[0]:[32]:[10.11.30.0]/88
                      10.11.30.0                                     0 65000 65013 i

Route Distinguisher: 10.10.30.0:32778
*>e[2]:[0]:[0]:[48]:[50af.2100.800b]:[0]:[0.0.0.0]/216
                      10.11.30.0                                     0 65000 65013 i
* e                   10.11.30.0                                     0 65000 65013 i
* e[2]:[0]:[0]:[48]:[50f2.9700.800b]:[0]:[0.0.0.0]/216
                      10.11.30.0                                     0 65000 65013 i
*>e                   10.11.30.0                                     0 65000 65013 i
* e[3]:[0]:[32]:[10.11.30.0]/88
                      10.11.30.0                                     0 65000 65013 i
*>e                   10.11.30.0                                     0 65000 65013 i

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
C   11     5000.5a00.800b   dynamic  NA         F      F    nve1(10.11.10.0)
C   11     505d.5100.800b   dynamic  NA         F      F    nve1(10.11.20.0)
*   11     50af.2100.800b   dynamic  NA         F      F    Eth1/3
*   11     50f2.9700.800b   dynamic  NA         F      F    Eth1/4
G    -     5000.0300.1b08   static   -         F      F    sup-eth1(R)
Leaf_3# show l2route mac topology 11

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
11          5000.5a00.800b BGP    Rcv                0          10.11.10.0 (Label: 10011)
11          505d.5100.800b BGP    Rcv                0          10.11.20.0 (Label: 10011)
11          50af.2100.800b Local  L,                 0          Eth1/3
11          50f2.9700.800b Local  L,                 0          Eth1/4
Leaf_3# show nve internal bgp rnh database
--------------------------------------------
Total peer-vni msgs recvd from bgp: 2
Peer add requests: 2
Peer update requests: 0
Peer delete requests: 0
Peer add/update requests: 2
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
10011     10.11.10.0         0000.0000.0000      0x0        vxlan     (1 /0 ) 0     FAB   SYM   10011      1            0            0
10011     10.11.20.0         0000.0000.0000      0x0        vxlan     (1 /0 ) 0     FAB   SYM   10011      1            0            0

Leaf_3# show bgp l2vpn evpn
BGP routing table information for VRF default, address family L2VPN EVPN
BGP table version is 22, Local Router ID is 10.10.30.0
Status: s-suppressed, x-deleted, S-stale, d-dampened, h-history, *-valid, >-best
Path type: i-internal, e-external, c-confed, l-local, a-aggregate, r-redist, I-injected
Origin codes: i - IGP, e - EGP, ? - incomplete, | - multipath, & - backup, 2 - best2

   Network            Next Hop            Metric     LocPrf     Weight Path
Route Distinguisher: 10.10.10.0:32778
* e[2]:[0]:[0]:[48]:[5000.5a00.800b]:[0]:[0.0.0.0]/216
                      10.11.10.0                                     0 65000 65011 i
*>e                   10.11.10.0                                     0 65000 65011 i
* e[3]:[0]:[32]:[10.11.10.0]/88
                      10.11.10.0                                     0 65000 65011 i
*>e                   10.11.10.0                                     0 65000 65011 i

Route Distinguisher: 10.10.20.0:32778
* e[2]:[0]:[0]:[48]:[505d.5100.800b]:[0]:[0.0.0.0]/216
                      10.11.20.0                                     0 65000 65012 i
*>e                   10.11.20.0                                     0 65000 65012 i
* e[3]:[0]:[32]:[10.11.20.0]/88
                      10.11.20.0                                     0 65000 65012 i
*>e                   10.11.20.0                                     0 65000 65012 i

Route Distinguisher: 10.10.30.0:32778    (L2VNI 10011)
*>e[2]:[0]:[0]:[48]:[5000.5a00.800b]:[0]:[0.0.0.0]/216
                      10.11.10.0                                     0 65000 65011 i
*>e[2]:[0]:[0]:[48]:[505d.5100.800b]:[0]:[0.0.0.0]/216
                      10.11.20.0                                     0 65000 65012 i
*>l[2]:[0]:[0]:[48]:[50af.2100.800b]:[0]:[0.0.0.0]/216
                      10.11.30.0                        100      32768 i
*>l[2]:[0]:[0]:[48]:[50f2.9700.800b]:[0]:[0.0.0.0]/216
                      10.11.30.0                        100      32768 i
*>e[3]:[0]:[32]:[10.11.10.0]/88
                      10.11.10.0                                     0 65000 65011 i
*>e[3]:[0]:[32]:[10.11.20.0]/88
                      10.11.20.0                                     0 65000 65012 i
*>l[3]:[0]:[32]:[10.11.30.0]/88
                      10.11.30.0                        100      32768 i

Leaf_3#
```
</details>

<details> 
<summary> Spine_1 </summary>

```
Spine_1# show bgp l2vpn evpn
BGP routing table information for VRF default, address family L2VPN EVPN
BGP table version is 17, Local Router ID is 10.10.1.0
Status: s-suppressed, x-deleted, S-stale, d-dampened, h-history, *-valid, >-best
Path type: i-internal, e-external, c-confed, l-local, a-aggregate, r-redist, I-injected
Origin codes: i - IGP, e - EGP, ? - incomplete, | - multipath, & - backup, 2 - best2

   Network            Next Hop            Metric     LocPrf     Weight Path
Route Distinguisher: 10.10.10.0:32778
*>e[2]:[0]:[0]:[48]:[5000.5a00.800b]:[0]:[0.0.0.0]/216
                      10.11.10.0                                     0 65011 i
*>e[3]:[0]:[32]:[10.11.10.0]/88
                      10.11.10.0                                     0 65011 i

Route Distinguisher: 10.10.20.0:32778
*>e[2]:[0]:[0]:[48]:[505d.5100.800b]:[0]:[0.0.0.0]/216
                      10.11.20.0                                     0 65012 i
*>e[3]:[0]:[32]:[10.11.20.0]/88
                      10.11.20.0                                     0 65012 i

Route Distinguisher: 10.10.30.0:32778
*>e[2]:[0]:[0]:[48]:[50af.2100.800b]:[0]:[0.0.0.0]/216
                      10.11.30.0                                     0 65013 i
*>e[2]:[0]:[0]:[48]:[50f2.9700.800b]:[0]:[0.0.0.0]/216
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
BGP table version is 17, Local Router ID is 10.10.2.0
Status: s-suppressed, x-deleted, S-stale, d-dampened, h-history, *-valid, >-best
Path type: i-internal, e-external, c-confed, l-local, a-aggregate, r-redist, I-injected
Origin codes: i - IGP, e - EGP, ? - incomplete, | - multipath, & - backup, 2 - best2

   Network            Next Hop            Metric     LocPrf     Weight Path
Route Distinguisher: 10.10.10.0:32778
*>e[2]:[0]:[0]:[48]:[5000.5a00.800b]:[0]:[0.0.0.0]/216
                      10.11.10.0                                     0 65011 i
*>e[3]:[0]:[32]:[10.11.10.0]/88
                      10.11.10.0                                     0 65011 i

Route Distinguisher: 10.10.20.0:32778
*>e[2]:[0]:[0]:[48]:[505d.5100.800b]:[0]:[0.0.0.0]/216
                      10.11.20.0                                     0 65012 i
*>e[3]:[0]:[32]:[10.11.20.0]/88
                      10.11.20.0                                     0 65012 i

Route Distinguisher: 10.10.30.0:32778
*>e[2]:[0]:[0]:[48]:[50af.2100.800b]:[0]:[0.0.0.0]/216
                      10.11.30.0                                     0 65013 i
*>e[2]:[0]:[0]:[48]:[50f2.9700.800b]:[0]:[0.0.0.0]/216
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
Host_1#ping 10.13.11.3
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.13.11.3, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 7/9/14 ms
Host_1#ping 10.13.11.4
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.13.11.4, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 9/10/12 ms
Host_1#ping 10.13.11.5
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.13.11.5, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 8/9/11 ms
Host_1#
```
