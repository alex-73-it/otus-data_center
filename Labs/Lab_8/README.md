# Адресное пространство и схема сети
![alt text](Схема_сети.png)

# Конфигурации устройств
<details>
<summary> Конфиг Host_1 </summary>

```
vlan 200
 name VL_EXT_CONNECT

interface Loopback200
 ip address 200.200.200.0 255.255.255.255
 ip ospf 1 area 0

interface Vlan200
 description INT_EXT_CONNECT
 ip address 10.13.200.3 255.255.255.0
 ip ospf 1 area 0

router ospf 1
 router-id 200.200.200.0
```
</details>

<details>
<summary> Конфиг Leaf_1 </summary>

```
vlan 200
  name VL_EXT_CONNECT

interface Vlan200
  description INT_EXT_CONNECT
  no shutdown
  vrf member l3vni
  ip address 10.13.200.1/24
  ip router ospf OSPF_EXT_CONNECT area 0.0.0.0

interface port-channel13
  description to_Host1
  switchport mode trunk
  switchport trunk allowed vlan 11,200
  vpc 13

interface port-channel56
  description vPC_PEER_LINKS
  switchport mode trunk
  switchport trunk allowed vlan 11,50,100,200
  spanning-tree port type network
  vpc peer-link

route-map permit-ospf-bgp permit 10

router ospf OSPF_EXT_CONNECT
  log-adjacency-changes
  vrf l3vni
    router-id 10.13.200.1
    log-adjacency-changes

router bgp 65011
  vrf l3vni
    address-family ipv4 unicast
      redistribute ospf OSPF_EXT_CONNECT route-map permit-ospf-bgp
```
</details>

<details>
<summary> Конфиг Leaf_2 </summary>

```
vlan 200
  name VL_EXT_CONNECT

interface Vlan200
  description INT_EXT_CONNECT
  no shutdown
  vrf member l3vni
  ip address 10.13.200.2/24
  ip router ospf OSPF_EXT_CONNECT area 0.0.0.0

interface port-channel13
  description to_Host1
  switchport mode trunk
  switchport trunk allowed vlan 11,200
  vpc 13

interface port-channel56
  description vPC_PEER_LINKS
  switchport mode trunk
  switchport trunk allowed vlan 11,50,100,200
  spanning-tree port type network
  vpc peer-link

route-map permit-ospf-bgp permit 10

router ospf OSPF_EXT_CONNECT
  log-adjacency-changes
  vrf l3vni
    router-id 10.13.200.2
    log-adjacency-changes

router bgp 65011
  vrf l3vni
    address-family ipv4 unicast
      redistribute ospf OSPF_EXT_CONNECT route-map permit-ospf-bgp
```
</details>

# Проверка работоспособности
1. Вывод команд show на устройствах
<details> 
<summary> Leaf_1 </summary>

```
Leaf_1# sh ip ospf neighbors vrf l3vni
 OSPF Process ID OSPF_EXT_CONNECT VRF l3vni
 Total number of neighbors: 2
 Neighbor ID     Pri State            Up Time  Address         Interface
 10.13.200.2       1 FULL/BDR         1w0d     10.13.200.2     Vlan200
 200.200.200.0     1 FULL/DROTHER     1w0d     10.13.200.3     Vlan200

Leaf_1# sho ip route ospf-oSPF_EXT_CONNECT vrf l3vni
IP Route Table for VRF "l3vni"
'*' denotes best ucast next-hop
'**' denotes best mcast next-hop
'[x/y]' denotes [preference/metric]
'%<string>' in via output denotes VRF <string>

200.200.200.0/32, ubest/mbest: 1/0
    *via 10.13.200.3, Vlan200, [110/41], 1w0d, ospf-OSPF_EXT_CONNECT, intra

Leaf_1# show bgp l2vpn evpn route-type 5 vrf l3vni
BGP routing table information for VRF default, address family L2VPN EVPN
Route Distinguisher: 10.10.10.0:4    (L3VNI 10100)
BGP routing table entry for [5]:[0]:[0]:[32]:[200.200.200.0]/224, version 3141
Paths: (1 available, best #1)
Flags: (0x000002) (high32 00000000) on xmit-list, is not in l2rib/evpn
Multipath: eBGP

  Advertised path-id 1
  Path type: local, path is valid, is best path, no labeled nexthop
  Gateway IP: 0.0.0.0
  AS-Path: NONE, path locally originated
    10.11.10.0 (metric 0) from 0.0.0.0 (10.10.10.0)
      Origin incomplete, MED 41, localpref 100, weight 32768
      Received label 10100
      Extcommunity: RT:65011:10100 ENCAP:8 Router MAC:5000.0100.1b08
          OSPF RT:0.0.0.0:0:0

  Path-id 1 advertised to peers:
    10.10.1.0          10.10.2.0

Leaf_1#
```
</details>

<details> 
<summary> Leaf_2 </summary>

```
Leaf_2# sh ip ospf neighbors vrf l3vni
 OSPF Process ID OSPF_EXT_CONNECT VRF l3vni
 Total number of neighbors: 2
 Neighbor ID     Pri State            Up Time  Address         Interface
 10.13.200.1       1 FULL/DR          1w0d     10.13.200.1     Vlan200
 200.200.200.0     1 FULL/DROTHER     1w0d     10.13.200.3     Vlan200

Leaf_2# sho ip route ospf-oSPF_EXT_CONNECT vrf l3vni
IP Route Table for VRF "l3vni"
'*' denotes best ucast next-hop
'**' denotes best mcast next-hop
'[x/y]' denotes [preference/metric]
'%<string>' in via output denotes VRF <string>

200.200.200.0/32, ubest/mbest: 1/0
    *via 10.13.200.3, Vlan200, [110/41], 1w0d, ospf-OSPF_EXT_CONNECT, intra

Leaf_2# show bgp l2vpn evpn route-type 5 vrf l3vni
BGP routing table information for VRF default, address family L2VPN EVPN
Route Distinguisher: 10.10.20.0:4    (L3VNI 10100)
BGP routing table entry for [5]:[0]:[0]:[32]:[200.200.200.0]/224, version 1794
Paths: (1 available, best #1)
Flags: (0x000002) (high32 00000000) on xmit-list, is not in l2rib/evpn
Multipath: eBGP

  Advertised path-id 1
  Path type: local, path is valid, is best path, no labeled nexthop
  Gateway IP: 0.0.0.0
  AS-Path: NONE, path locally originated
    10.11.20.0 (metric 0) from 0.0.0.0 (10.10.20.0)
      Origin incomplete, MED 41, localpref 100, weight 32768
      Received label 10100
      Extcommunity: RT:65011:10100 ENCAP:8 Router MAC:5000.0200.1b08
          OSPF RT:0.0.0.0:0:0

  Path-id 1 advertised to peers:
    10.10.1.0          10.10.2.0

Leaf_2#
```
</details>

<details> 
<summary> Leaf_3 </summary>

```
Leaf_3# show bgp l2vpn evpn route-type 5 vrf l3vni
BGP routing table information for VRF default, address family L2VPN EVPN
Route Distinguisher: 10.10.30.0:4    (L3VNI 10100)
BGP routing table entry for [5]:[0]:[0]:[32]:[200.200.200.0]/224, version 6377
Paths: (2 available, best #2)
Flags: (0x000002) (high32 00000000) on xmit-list, is not in l2rib/evpn, is not in HW
Multipath: eBGP

  Path type: external, path is valid, not best reason: newer EBGP path, multipath, no labeled nexthop
             Imported from 10.10.10.0:4:[5]:[0]:[0]:[32]:[200.200.200.0]/224
  Gateway IP: 0.0.0.0
  AS-Path: 65000 65011 , path sourced external to AS
    10.11.10.0 (metric 81) from 10.10.1.0 (10.10.1.0)
      Origin incomplete, MED not set, localpref 100, weight 0
      Received label 10100
      Extcommunity: RT:65013:10100 ENCAP:8 Router MAC:5000.0100.1b08
          OSPF RT:0.0.0.0:0:0

  Advertised path-id 1
  Path type: external, path is valid, is best path, no labeled nexthop
             Imported from 10.10.20.0:4:[5]:[0]:[0]:[32]:[200.200.200.0]/224
  Gateway IP: 0.0.0.0
  AS-Path: 65000 65011 , path sourced external to AS
    10.11.20.0 (metric 81) from 10.10.2.0 (10.10.2.0)
      Origin incomplete, MED not set, localpref 100, weight 0
      Received label 10100
      Extcommunity: RT:65013:10100 ENCAP:8 Router MAC:5000.0200.1b08
          OSPF RT:0.0.0.0:0:0

  Path-id 1 not advertised to any peer

Leaf_3#
```
</details>

<details> 
<summary> Leaf_4 </summary>

```
Leaf_4# show bgp l2vpn evpn route-type 5 vrf l3vni
BGP routing table information for VRF default, address family L2VPN EVPN
Route Distinguisher: 10.10.40.0:4    (L3VNI 10100)
BGP routing table entry for [5]:[0]:[0]:[32]:[200.200.200.0]/224, version 2198
Paths: (2 available, best #2)
Flags: (0x000002) (high32 00000000) on xmit-list, is not in l2rib/evpn, is not in HW
Multipath: eBGP

  Path type: external, path is valid, not best reason: newer EBGP path, multipath, no labeled nexthop
             Imported from 10.10.10.0:4:[5]:[0]:[0]:[32]:[200.200.200.0]/224
  Gateway IP: 0.0.0.0
  AS-Path: 65000 65011 , path sourced external to AS
    10.11.10.0 (metric 81) from 10.10.1.0 (10.10.1.0)
      Origin incomplete, MED not set, localpref 100, weight 0
      Received label 10100
      Extcommunity: RT:65014:10100 ENCAP:8 Router MAC:5000.0100.1b08
          OSPF RT:0.0.0.0:0:0

  Advertised path-id 1
  Path type: external, path is valid, is best path, no labeled nexthop
             Imported from 10.10.20.0:4:[5]:[0]:[0]:[32]:[200.200.200.0]/224
  Gateway IP: 0.0.0.0
  AS-Path: 65000 65011 , path sourced external to AS
    10.11.20.0 (metric 81) from 10.10.1.0 (10.10.1.0)
      Origin incomplete, MED not set, localpref 100, weight 0
      Received label 10100
      Extcommunity: RT:65014:10100 ENCAP:8 Router MAC:5000.0200.1b08
          OSPF RT:0.0.0.0:0:0

  Path-id 1 not advertised to any peer

Leaf_4#
```
</details>

2. Проверка доступности сетей c помощью ping
<details>
<summary> ping с Host_2 до внешней сети 200.200.200.0 </summary>

```
Host_2#ping 200.200.200.0
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 200.200.200.0, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 12/15/18 ms
Host_2#
```
</details>

<details> 
<summary> ping с Host_3 до внешней сети 200.200.200.0 </summary>

```
Host_3#ping 200.200.200.0
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 200.200.200.0, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 10/13/17 ms
Host_3#
```
</details>