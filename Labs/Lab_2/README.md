# Адресное пространство и схема сети
![](<Схема сети.png>)
# Конфигурации устройств
<details> 
<summary> Конфиг Leaf_1 </summary> 

```
feature ospf
feature interface-vlan

vlan 1,11

key chain ospf
  key 0
    key-string 7 0700325c48
    cryptographic-algorithm HMAC-SHA-256

interface Vlan11
  no shutdown
  ip address 10.13.11.1/24
  ip ospf cost 1
  ip ospf network point-to-point
  ip ospf passive-interface
  ip router ospf 0 area 0.0.0.0

interface Ethernet1/1
  no switchport
  ip address 10.12.11.0/31
  ip ospf authentication key-chain ospf
  ip ospf network point-to-point
  no ip ospf passive-interface
  ip router ospf 0 area 0.0.0.0
  no shutdown

interface Ethernet1/2
  no switchport
  ip address 10.12.12.0/31
  ip ospf authentication key-chain ospf
  ip ospf network point-to-point
  no ip ospf passive-interface
  ip router ospf 0 area 0.0.0.0
  no shutdown

interface Ethernet1/3
  switchport access vlan 11

interface loopback0
  ip address 10.10.10.0/32
  ip router ospf 0 area 0.0.0.0

router ospf 0
  router-id 10.10.10.0
  log-adjacency-changes
  area 0.0.0.0 authentication message-digest
  passive-interface default
```
</details> 

<details> 
<summary> Конфиг Leaf_2 </summary>

```
feature ospf
feature interface-vlan

vlan 1,22

key chain ospf
  key 0
    key-string 7 0700325c48
    cryptographic-algorithm HMAC-SHA-256

interface Vlan22
  no shutdown
  ip address 10.13.22.1/24
  ip ospf cost 1
  ip ospf network point-to-point
  ip ospf passive-interface
  ip router ospf 0 area 0.0.0.0

interface Ethernet1/1
  no switchport
  ip address 10.12.21.0/31
  ip ospf authentication key-chain ospf
  ip ospf network point-to-point
  no ip ospf passive-interface
  ip router ospf 0 area 0.0.0.0
  no shutdown

interface Ethernet1/2
  no switchport
  ip address 10.12.22.0/31
  ip ospf authentication key-chain ospf
  ip ospf network point-to-point
  no ip ospf passive-interface
  ip router ospf 0 area 0.0.0.0
  no shutdown

interface Ethernet1/3
  switchport access vlan 22

interface loopback0
  ip address 10.10.20.0/32
  ip router ospf 0 area 0.0.0.0

router ospf 0
  router-id 10.10.20.0
  log-adjacency-changes
  area 0.0.0.0 authentication message-digest
  passive-interface default
```
</details> 

<details> 
<summary> Конфиг Leaf_3 </summary>

```
feature ospf
feature interface-vlan

vlan 1,33-34

key chain ospf
  key 0
    key-string 7 0700325c48
    cryptographic-algorithm HMAC-SHA-256

interface Vlan33
  no shutdown
  ip address 10.13.33.1/24
  ip ospf cost 1
  ip ospf network point-to-point
  ip ospf passive-interface
  ip router ospf 0 area 0.0.0.0

interface Vlan34
  no shutdown
  ip address 10.13.34.1/24
  ip ospf cost 1
  ip ospf network point-to-point
  ip ospf passive-interface
  ip router ospf 0 area 0.0.0.0

interface Ethernet1/1
  no switchport
  ip address 10.12.31.0/31
  ip ospf authentication key-chain ospf
  ip ospf network point-to-point
  no ip ospf passive-interface
  ip router ospf 0 area 0.0.0.0
  no shutdown

interface Ethernet1/2
  no switchport
  ip address 10.12.32.0/31
  ip ospf authentication key-chain ospf
  ip ospf network point-to-point
  no ip ospf passive-interface
  ip router ospf 0 area 0.0.0.0
  no shutdown

interface Ethernet1/3
  switchport access vlan 33

interface Ethernet1/4
  switchport access vlan 34

interface loopback0
  ip address 10.10.30.0/32
  ip router ospf 0 area 0.0.0.0

router ospf 0
  router-id 10.10.30.0
  log-adjacency-changes
  area 0.0.0.0 authentication message-digest
  passive-interface default
```
</details> 

<details> 
<summary> Конфиг Spine_1 </summary>

```
feature ospf

key chain ospf
  key 0
    key-string 7 0700325c48
    cryptographic-algorithm HMAC-SHA-256

interface Ethernet1/1
  no switchport
  ip address 10.12.11.1/31
  ip ospf authentication key-chain ospf
  ip ospf network point-to-point
  no ip ospf passive-interface
  ip router ospf 0 area 0.0.0.0
  no shutdown

interface Ethernet1/2
  no switchport
  ip address 10.12.21.1/31
  ip ospf authentication key-chain ospf
  ip ospf network point-to-point
  no ip ospf passive-interface
  ip router ospf 0 area 0.0.0.0
  no shutdown

interface Ethernet1/3
  no switchport
  ip address 10.12.31.1/31
  ip ospf authentication key-chain ospf
  ip ospf network point-to-point
  no ip ospf passive-interface
  ip router ospf 0 area 0.0.0.0
  no shutdown

interface loopback0
  ip address 10.10.1.0/32
  ip router ospf 0 area 0.0.0.0

router ospf 0
  router-id 10.10.1.0
  log-adjacency-changes
  area 0.0.0.0 authentication message-digest
  passive-interface default
```
</details>

<details> 
<summary> Конфиг Spine_2 </summary>

```
feature ospf

key chain ospf
  key 0
    key-string 7 0700325c48
    cryptographic-algorithm HMAC-SHA-256

interface Ethernet1/1
  no switchport
  ip address 10.12.12.1/31
  ip ospf authentication key-chain ospf
  ip ospf network point-to-point
  no ip ospf passive-interface
  ip router ospf 0 area 0.0.0.0
  no shutdown

interface Ethernet1/2
  no switchport
  ip address 10.12.22.1/31
  ip ospf authentication key-chain ospf
  ip ospf network point-to-point
  no ip ospf passive-interface
  ip router ospf 0 area 0.0.0.0
  no shutdown

interface Ethernet1/3
  no switchport
  ip address 10.12.32.1/31
  ip ospf authentication key-chain ospf
  ip ospf network point-to-point
  no ip ospf passive-interface
  ip router ospf 0 area 0.0.0.0
  no shutdown

interface loopback0
  ip address 10.10.2.0/32
  ip router ospf 0 area 0.0.0.0

router ospf 0
  router-id 10.10.2.0
  log-adjacency-changes
  area 0.0.0.0 authentication message-digest
  passive-interface default
```
</details>

# Проверка работоспособности
1. Вывод команд show на устройствах
<details> 
<summary> Leaf_1 </summary>

```
Leaf_1# sho ip ospf neighbors
 OSPF Process ID 0 VRF default
 Total number of neighbors: 2
 Neighbor ID     Pri State            Up Time  Address         Interface
 10.10.1.0         1 FULL/ -          03:51:48 10.12.11.1      Eth1/1
 10.10.2.0         1 FULL/ -          03:36:24 10.12.12.1      Eth1/2
Leaf_1# sho ip route ospf-0
IP Route Table for VRF "default"
'*' denotes best ucast next-hop
'**' denotes best mcast next-hop
'[x/y]' denotes [preference/metric]
'%<string>' in via output denotes VRF <string>

10.10.1.0/32, ubest/mbest: 1/0
    *via 10.12.11.1, Eth1/1, [110/41], 03:57:38, ospf-0, intra
10.10.2.0/32, ubest/mbest: 1/0
    *via 10.12.12.1, Eth1/2, [110/41], 03:36:31, ospf-0, intra
10.10.20.0/32, ubest/mbest: 2/0
    *via 10.12.11.1, Eth1/1, [110/81], 03:51:49, ospf-0, intra
    *via 10.12.12.1, Eth1/2, [110/81], 03:36:31, ospf-0, intra
10.10.30.0/32, ubest/mbest: 2/0
    *via 10.12.11.1, Eth1/1, [110/81], 03:51:53, ospf-0, intra
    *via 10.12.12.1, Eth1/2, [110/81], 03:36:31, ospf-0, intra
10.12.21.0/31, ubest/mbest: 1/0
    *via 10.12.11.1, Eth1/1, [110/80], 03:57:38, ospf-0, intra
10.12.22.0/31, ubest/mbest: 1/0
    *via 10.12.12.1, Eth1/2, [110/80], 03:36:31, ospf-0, intra
10.12.31.0/31, ubest/mbest: 1/0
    *via 10.12.11.1, Eth1/1, [110/80], 03:57:38, ospf-0, intra
10.12.32.0/31, ubest/mbest: 1/0
    *via 10.12.12.1, Eth1/2, [110/80], 03:36:31, ospf-0, intra
10.13.22.0/24, ubest/mbest: 2/0
    *via 10.12.11.1, Eth1/1, [110/81], 03:51:49, ospf-0, intra
    *via 10.12.12.1, Eth1/2, [110/81], 03:36:31, ospf-0, intra
10.13.33.0/24, ubest/mbest: 2/0
    *via 10.12.11.1, Eth1/1, [110/81], 03:51:53, ospf-0, intra
    *via 10.12.12.1, Eth1/2, [110/81], 03:36:31, ospf-0, intra
10.13.34.0/24, ubest/mbest: 2/0
    *via 10.12.11.1, Eth1/1, [110/81], 03:51:53, ospf-0, intra
    *via 10.12.12.1, Eth1/2, [110/81], 03:36:31, ospf-0, intra

Leaf_1# show ip ospf database
        OSPF Router with ID (10.10.10.0) (Process ID 0 VRF default)

                Router Link States (Area 0.0.0.0)

Link ID         ADV Router      Age        Seq#       Checksum Link Count
10.10.1.0       10.10.1.0       1523       0x80000040 0x7993   7
10.10.2.0       10.10.2.0       265        0x80000042 0xab56   7
10.10.10.0      10.10.10.0      268        0x8000003a 0x9866   6
10.10.20.0      10.10.20.0      1185       0x8000003e 0x5653   6
10.10.30.0      10.10.30.0      1187       0x8000003d 0x27e7   7

Leaf_1#
```
</details>

<details> 
<summary> Leaf_2 </summary>

```
Leaf_2# show ip ospf neighbors
 OSPF Process ID 0 VRF default
 Total number of neighbors: 2
 Neighbor ID     Pri State            Up Time  Address         Interface
 10.10.1.0         1 FULL/ -          03:54:59 10.12.21.1      Eth1/1
 10.10.2.0         1 FULL/ -          03:55:03 10.12.22.1      Eth1/2
Leaf_2# sho ip route ospf-0
IP Route Table for VRF "default"
'*' denotes best ucast next-hop
'**' denotes best mcast next-hop
'[x/y]' denotes [preference/metric]
'%<string>' in via output denotes VRF <string>

10.10.1.0/32, ubest/mbest: 1/0
    *via 10.12.21.1, Eth1/1, [110/41], 04:00:46, ospf-0, intra
10.10.2.0/32, ubest/mbest: 1/0
    *via 10.12.22.1, Eth1/2, [110/41], 03:55:06, ospf-0, intra
10.10.10.0/32, ubest/mbest: 2/0
    *via 10.12.21.1, Eth1/1, [110/81], 04:00:45, ospf-0, intra
    *via 10.12.22.1, Eth1/2, [110/81], 03:39:39, ospf-0, intra
10.10.30.0/32, ubest/mbest: 2/0
    *via 10.12.21.1, Eth1/1, [110/81], 03:55:01, ospf-0, intra
    *via 10.12.22.1, Eth1/2, [110/81], 03:55:01, ospf-0, intra
10.12.11.0/31, ubest/mbest: 1/0
    *via 10.12.21.1, Eth1/1, [110/80], 04:00:46, ospf-0, intra
10.12.12.0/31, ubest/mbest: 1/0
    *via 10.12.22.1, Eth1/2, [110/80], 03:55:06, ospf-0, intra
10.12.31.0/31, ubest/mbest: 1/0
    *via 10.12.21.1, Eth1/1, [110/80], 04:00:46, ospf-0, intra
10.12.32.0/31, ubest/mbest: 1/0
    *via 10.12.22.1, Eth1/2, [110/80], 03:55:06, ospf-0, intra
10.13.11.0/24, ubest/mbest: 2/0
    *via 10.12.21.1, Eth1/1, [110/81], 04:00:45, ospf-0, intra
    *via 10.12.22.1, Eth1/2, [110/81], 03:39:39, ospf-0, intra
10.13.33.0/24, ubest/mbest: 2/0
    *via 10.12.21.1, Eth1/1, [110/81], 03:55:01, ospf-0, intra
    *via 10.12.22.1, Eth1/2, [110/81], 03:55:01, ospf-0, intra
10.13.34.0/24, ubest/mbest: 2/0
    *via 10.12.21.1, Eth1/1, [110/81], 03:55:01, ospf-0, intra
    *via 10.12.22.1, Eth1/2, [110/81], 03:55:01, ospf-0, intra

Leaf_2# show ip ospf database
        OSPF Router with ID (10.10.20.0) (Process ID 0 VRF default)

                Router Link States (Area 0.0.0.0)

Link ID         ADV Router      Age        Seq#       Checksum Link Count
10.10.1.0       10.10.1.0       1709       0x80000040 0x7993   7
10.10.2.0       10.10.2.0       450        0x80000042 0xab56   7
10.10.10.0      10.10.10.0      456        0x8000003a 0x9866   6
10.10.20.0      10.10.20.0      1369       0x8000003e 0x5653   6
10.10.30.0      10.10.30.0      1373       0x8000003d 0x27e7   7

Leaf_2#
```
</details>

<details> 
<summary> Leaf_3 </summary>

```
Leaf_3# show ip ospf neighbors
 OSPF Process ID 0 VRF default
 Total number of neighbors: 2
 Neighbor ID     Pri State            Up Time  Address         Interface
 10.10.1.0         1 FULL/ -          03:55:43 10.12.31.1      Eth1/1
 10.10.2.0         1 FULL/ -          03:55:45 10.12.32.1      Eth1/2
Leaf_3# sh ip route ospf-0
IP Route Table for VRF "default"
'*' denotes best ucast next-hop
'**' denotes best mcast next-hop
'[x/y]' denotes [preference/metric]
'%<string>' in via output denotes VRF <string>

10.10.1.0/32, ubest/mbest: 1/0
    *via 10.12.31.1, Eth1/1, [110/41], 03:55:45, ospf-0, intra
10.10.2.0/32, ubest/mbest: 1/0
    *via 10.12.32.1, Eth1/2, [110/41], 03:55:45, ospf-0, intra
10.10.10.0/32, ubest/mbest: 2/0
    *via 10.12.31.1, Eth1/1, [110/81], 03:55:45, ospf-0, intra
    *via 10.12.32.1, Eth1/2, [110/81], 03:40:23, ospf-0, intra
10.10.20.0/32, ubest/mbest: 2/0
    *via 10.12.31.1, Eth1/1, [110/81], 03:55:41, ospf-0, intra
    *via 10.12.32.1, Eth1/2, [110/81], 03:55:45, ospf-0, intra
10.12.11.0/31, ubest/mbest: 1/0
    *via 10.12.31.1, Eth1/1, [110/80], 03:55:45, ospf-0, intra
10.12.12.0/31, ubest/mbest: 1/0
    *via 10.12.32.1, Eth1/2, [110/80], 03:55:45, ospf-0, intra
10.12.21.0/31, ubest/mbest: 1/0
    *via 10.12.31.1, Eth1/1, [110/80], 03:55:45, ospf-0, intra
10.12.22.0/31, ubest/mbest: 1/0
    *via 10.12.32.1, Eth1/2, [110/80], 03:55:45, ospf-0, intra
10.13.11.0/24, ubest/mbest: 2/0
    *via 10.12.31.1, Eth1/1, [110/81], 03:55:45, ospf-0, intra
    *via 10.12.32.1, Eth1/2, [110/81], 03:40:23, ospf-0, intra
10.13.22.0/24, ubest/mbest: 2/0
    *via 10.12.31.1, Eth1/1, [110/81], 03:55:41, ospf-0, intra
    *via 10.12.32.1, Eth1/2, [110/81], 03:55:45, ospf-0, intra

Leaf_3# show ip ospf database
        OSPF Router with ID (10.10.30.0) (Process ID 0 VRF default)

                Router Link States (Area 0.0.0.0)

Link ID         ADV Router      Age        Seq#       Checksum Link Count
10.10.1.0       10.10.1.0       1752       0x80000040 0x7993   7
10.10.2.0       10.10.2.0       494        0x80000042 0xab56   7
10.10.10.0      10.10.10.0      499        0x8000003a 0x9866   6
10.10.20.0      10.10.20.0      1415       0x8000003e 0x5653   6
10.10.30.0      10.10.30.0      1414       0x8000003d 0x27e7   7

Leaf_3#
```
</details>

<details> 
<summary> Spine_1 </summary>

```
Spine_1# show ip ospf neighbors
 OSPF Process ID 0 VRF default
 Total number of neighbors: 3
 Neighbor ID     Pri State            Up Time  Address         Interface
 10.10.10.0        1 FULL/ -          03:56:41 10.12.11.0      Eth1/1
 10.10.20.0        1 FULL/ -          03:56:41 10.12.21.0      Eth1/2
 10.10.30.0        1 FULL/ -          03:56:41 10.12.31.0      Eth1/3
Spine_1# show ip route ospf-0
IP Route Table for VRF "default"
'*' denotes best ucast next-hop
'**' denotes best mcast next-hop
'[x/y]' denotes [preference/metric]
'%<string>' in via output denotes VRF <string>

10.10.2.0/32, ubest/mbest: 3/0
    *via 10.12.11.0, Eth1/1, [110/81], 03:41:22, ospf-0, intra
    *via 10.12.21.0, Eth1/2, [110/81], 03:56:39, ospf-0, intra
    *via 10.12.31.0, Eth1/3, [110/81], 03:56:44, ospf-0, intra
10.10.10.0/32, ubest/mbest: 1/0
    *via 10.12.11.0, Eth1/1, [110/41], 04:02:27, ospf-0, intra
10.10.20.0/32, ubest/mbest: 1/0
    *via 10.12.21.0, Eth1/2, [110/41], 03:56:39, ospf-0, intra
10.10.30.0/32, ubest/mbest: 1/0
    *via 10.12.31.0, Eth1/3, [110/41], 03:56:44, ospf-0, intra
10.12.12.0/31, ubest/mbest: 1/0
    *via 10.12.11.0, Eth1/1, [110/80], 04:02:27, ospf-0, intra
10.12.22.0/31, ubest/mbest: 1/0
    *via 10.12.21.0, Eth1/2, [110/80], 03:56:39, ospf-0, intra
10.12.32.0/31, ubest/mbest: 1/0
    *via 10.12.31.0, Eth1/3, [110/80], 03:56:44, ospf-0, intra
10.13.11.0/24, ubest/mbest: 1/0
    *via 10.12.11.0, Eth1/1, [110/41], 04:02:27, ospf-0, intra
10.13.22.0/24, ubest/mbest: 1/0
    *via 10.12.21.0, Eth1/2, [110/41], 03:56:39, ospf-0, intra
10.13.33.0/24, ubest/mbest: 1/0
    *via 10.12.31.0, Eth1/3, [110/41], 03:56:44, ospf-0, intra
10.13.34.0/24, ubest/mbest: 1/0
    *via 10.12.31.0, Eth1/3, [110/41], 03:56:44, ospf-0, intra

Spine_1# show ip ospf database
        OSPF Router with ID (10.10.1.0) (Process ID 0 VRF default)

                Router Link States (Area 0.0.0.0)

Link ID         ADV Router      Age        Seq#       Checksum Link Count
10.10.1.0       10.10.1.0       1810       0x80000040 0x7993   7
10.10.2.0       10.10.2.0       554        0x80000042 0xab56   7
10.10.10.0      10.10.10.0      557        0x8000003a 0x9866   6
10.10.20.0      10.10.20.0      1472       0x8000003e 0x5653   6
10.10.30.0      10.10.30.0      1474       0x8000003d 0x27e7   7

Spine_1#
```
</details>

<details> 
<summary> Spine_2 </summary>

```
Spine_2# show ip ospf neighbors
 OSPF Process ID 0 VRF default
 Total number of neighbors: 3
 Neighbor ID     Pri State            Up Time  Address         Interface
 10.10.10.0        1 FULL/ -          03:42:02 10.12.12.0      Eth1/1
 10.10.20.0        1 FULL/ -          03:57:31 10.12.22.0      Eth1/2
 10.10.30.0        1 FULL/ -          03:57:29 10.12.32.0      Eth1/3
Spine_2# show ip route ospf-0
IP Route Table for VRF "default"
'*' denotes best ucast next-hop
'**' denotes best mcast next-hop
'[x/y]' denotes [preference/metric]
'%<string>' in via output denotes VRF <string>

10.10.1.0/32, ubest/mbest: 3/0
    *via 10.12.12.0, Eth1/1, [110/81], 03:42:11, ospf-0, intra
    *via 10.12.22.0, Eth1/2, [110/81], 03:57:28, ospf-0, intra
    *via 10.12.32.0, Eth1/3, [110/81], 03:57:32, ospf-0, intra
10.10.10.0/32, ubest/mbest: 1/0
    *via 10.12.12.0, Eth1/1, [110/41], 03:42:11, ospf-0, intra
10.10.20.0/32, ubest/mbest: 1/0
    *via 10.12.22.0, Eth1/2, [110/41], 03:57:43, ospf-0, intra
10.10.30.0/32, ubest/mbest: 1/0
    *via 10.12.32.0, Eth1/3, [110/41], 03:57:32, ospf-0, intra
10.12.11.0/31, ubest/mbest: 1/0
    *via 10.12.12.0, Eth1/1, [110/80], 03:42:11, ospf-0, intra
10.12.21.0/31, ubest/mbest: 1/0
    *via 10.12.22.0, Eth1/2, [110/80], 03:57:43, ospf-0, intra
10.12.31.0/31, ubest/mbest: 1/0
    *via 10.12.32.0, Eth1/3, [110/80], 03:57:32, ospf-0, intra
10.13.11.0/24, ubest/mbest: 1/0
    *via 10.12.12.0, Eth1/1, [110/41], 03:42:11, ospf-0, intra
10.13.22.0/24, ubest/mbest: 1/0
    *via 10.12.22.0, Eth1/2, [110/41], 03:57:43, ospf-0, intra
10.13.33.0/24, ubest/mbest: 1/0
    *via 10.12.32.0, Eth1/3, [110/41], 03:57:32, ospf-0, intra
10.13.34.0/24, ubest/mbest: 1/0
    *via 10.12.32.0, Eth1/3, [110/41], 03:57:32, ospf-0, intra

Spine_2# show ip ospf database
        OSPF Router with ID (10.10.2.0) (Process ID 0 VRF default)

                Router Link States (Area 0.0.0.0)

Link ID         ADV Router      Age        Seq#       Checksum Link Count
10.10.1.0       10.10.1.0       40         0x80000041 0x7794   7
10.10.2.0       10.10.2.0       600        0x80000042 0xab56   7
10.10.10.0      10.10.10.0      605        0x8000003a 0x9866   6
10.10.20.0      10.10.20.0      1520       0x8000003e 0x5653   6
10.10.30.0      10.10.30.0      1522       0x8000003d 0x27e7   7

Spine_2#
```
</details>

2. Проверка доступности сетей c помощью ping
<details>
<summary> ping с PC_1 до осатльных PC </summary>

```
pc_1> ping 10.13.22.2

10.13.22.2 icmp_seq=1 timeout
84 bytes from 10.13.22.2 icmp_seq=2 ttl=61 time=18.519 ms
84 bytes from 10.13.22.2 icmp_seq=3 ttl=61 time=12.888 ms
84 bytes from 10.13.22.2 icmp_seq=4 ttl=61 time=11.336 ms
^C
pc_1> ping 10.13.33.2

10.13.33.2 icmp_seq=1 timeout
84 bytes from 10.13.33.2 icmp_seq=2 ttl=61 time=14.836 ms
84 bytes from 10.13.33.2 icmp_seq=3 ttl=61 time=10.543 ms
84 bytes from 10.13.33.2 icmp_seq=4 ttl=61 time=13.305 ms
^C
pc_1> ping 10.13.34.2

10.13.34.2 icmp_seq=1 timeout
84 bytes from 10.13.34.2 icmp_seq=2 ttl=61 time=15.642 ms
84 bytes from 10.13.34.2 icmp_seq=3 ttl=61 time=15.050 ms
84 bytes from 10.13.34.2 icmp_seq=4 ttl=61 time=17.018 ms
^C
pc_1>
```
</detail>