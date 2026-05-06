# Адресное пространство и схема сети
![](<Снимок экрана 2026-05-06 в 18.28.14.png>)
# Конфигурации устройств
<details> 
<summary> Конфиг Leaf_1 </summary> 

```
feature ospf

interface Ethernet1/1
  no switchport
  ip address 10.12.11.0/31
  ip ospf network point-to-point
  ip router ospf 0 area 0.0.0.0
  no shutdown

interface Ethernet1/2
  no switchport
  ip address 10.12.12.0/31
  ip ospf network point-to-point
  ip router ospf 0 area 0.0.0.0
  no shutdown

interface loopback0
  ip address 10.10.10.0/32
  ip router ospf 0 area 0.0.0.0

router ospf 0
```
</details> 

<details> 
<summary> Конфиг Leaf_2 </summary>

```
feature ospf

interface Ethernet1/1
  no switchport
  ip address 10.12.21.0/31
  ip ospf network point-to-point
  ip router ospf 0 area 0.0.0.0
  no shutdown

interface Ethernet1/2
  no switchport
  ip address 10.12.22.0/31
  ip ospf network point-to-point
  ip router ospf 0 area 0.0.0.0
  no shutdown

interface loopback0
  ip address 10.10.20.0/32
  ip router ospf 0 area 0.0.0.0

router ospf 0
```
</details> 

<details> 
<summary> Конфиг Leaf_3 </summary>

```
feature ospf

interface Ethernet1/1
  no switchport
  ip address 10.12.31.0/31
  ip ospf network point-to-point
  ip router ospf 0 area 0.0.0.0
  no shutdown

interface Ethernet1/2
  no switchport
  ip address 10.12.32.0/31
  ip ospf network point-to-point
  ip router ospf 0 area 0.0.0.0
  no shutdown

interface loopback0
  ip address 10.10.30.0/32
  ip router ospf 0 area 0.0.0.0

router ospf 0
```
</details> 

<details> 
<summary> Конфиг Spine_1 </summary>

```
feature ospf

interface Ethernet1/1
  no switchport
  ip address 10.12.11.1/31
  ip ospf network point-to-point
  ip router ospf 0 area 0.0.0.0
  no shutdown

interface Ethernet1/2
  no switchport
  ip address 10.12.21.1/31
  ip ospf network point-to-point
  ip router ospf 0 area 0.0.0.0
  no shutdown

interface Ethernet1/3
  no switchport
  ip address 10.12.31.1/31
  ip ospf network point-to-point
  ip router ospf 0 area 0.0.0.0
  no shutdown

interface loopback0
  ip address 10.10.1.0/32
  ip router ospf 0 area 0.0.0.0

router ospf 0
```
</details>

<details> 
<summary> Конфиг Spine_2 </summary>

```
feature ospf

interface Ethernet1/1
  no switchport
  ip address 10.12.12.1/31
  ip ospf network point-to-point
  ip router ospf 0 area 0.0.0.0
  no shutdown

interface Ethernet1/2
  no switchport
  ip address 10.12.22.1/31
  ip ospf network point-to-point
  ip router ospf 0 area 0.0.0.0
  no shutdown

interface Ethernet1/3
  no switchport
  ip address 10.12.32.1/31
  ip ospf network point-to-point
  ip router ospf 0 area 0.0.0.0
  no shutdown

interface loopback0
  ip address 10.10.2.0/32
  ip router ospf 0 area 0.0.0.0

router ospf 0
```
</details>

# Проверка работоспособности
<details> 
<summary> Leaf_1 </summary>

```
Leaf_1# sho ip ospf neighbors
 OSPF Process ID 0 VRF default
 Total number of neighbors: 2
 Neighbor ID     Pri State            Up Time  Address         Interface
 10.10.1.0         1 FULL/ -          02:04:33 10.12.11.1      Eth1/1
 10.10.2.0         1 FULL/ -          01:23:13 10.12.12.1      Eth1/2
Leaf_1# sho ip route ospf-0
IP Route Table for VRF "default"
'*' denotes best ucast next-hop
'**' denotes best mcast next-hop
'[x/y]' denotes [preference/metric]
'%<string>' in via output denotes VRF <string>

10.10.1.0/32, ubest/mbest: 1/0
    *via 10.12.11.1, Eth1/1, [110/41], 02:04:35, ospf-0, intra
10.10.2.0/32, ubest/mbest: 1/0
    *via 10.12.12.1, Eth1/2, [110/41], 01:23:15, ospf-0, intra
10.10.20.0/32, ubest/mbest: 2/0
    *via 10.12.11.1, Eth1/1, [110/81], 01:07:37, ospf-0, intra
    *via 10.12.12.1, Eth1/2, [110/81], 01:13:21, ospf-0, intra
10.10.30.0/32, ubest/mbest: 2/0
    *via 10.12.11.1, Eth1/1, [110/81], 02:01:55, ospf-0, intra
    *via 10.12.12.1, Eth1/2, [110/81], 01:22:25, ospf-0, intra
10.12.21.0/31, ubest/mbest: 1/0
    *via 10.12.11.1, Eth1/1, [110/80], 01:08:22, ospf-0, intra
10.12.22.0/31, ubest/mbest: 1/0
    *via 10.12.12.1, Eth1/2, [110/80], 01:23:02, ospf-0, intra
10.12.31.0/31, ubest/mbest: 1/0
    *via 10.12.11.1, Eth1/1, [110/80], 02:02:07, ospf-0, intra
10.12.32.0/31, ubest/mbest: 1/0
    *via 10.12.12.1, Eth1/2, [110/80], 01:22:32, ospf-0, intra

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
 10.10.1.0         1 FULL/ -          01:10:52 10.12.21.1      Eth1/1
 10.10.2.0         1 FULL/ -          01:16:41 10.12.22.1      Eth1/2
Leaf_2# show ip route ospf-0
IP Route Table for VRF "default"
'*' denotes best ucast next-hop
'**' denotes best mcast next-hop
'[x/y]' denotes [preference/metric]
'%<string>' in via output denotes VRF <string>

10.10.1.0/32, ubest/mbest: 1/0
    *via 10.12.21.1, Eth1/1, [110/41], 01:10:56, ospf-0, intra
10.10.2.0/32, ubest/mbest: 1/0
    *via 10.12.22.1, Eth1/2, [110/41], 01:16:40, ospf-0, intra
10.10.10.0/32, ubest/mbest: 2/0
    *via 10.12.21.1, Eth1/1, [110/81], 01:10:56, ospf-0, intra
    *via 10.12.22.1, Eth1/2, [110/81], 01:16:40, ospf-0, intra
10.10.30.0/32, ubest/mbest: 2/0
    *via 10.12.21.1, Eth1/1, [110/81], 01:10:56, ospf-0, intra
    *via 10.12.22.1, Eth1/2, [110/81], 01:16:40, ospf-0, intra
10.12.11.0/31, ubest/mbest: 1/0
    *via 10.12.21.1, Eth1/1, [110/80], 01:10:56, ospf-0, intra
10.12.12.0/31, ubest/mbest: 1/0
    *via 10.12.22.1, Eth1/2, [110/80], 01:16:40, ospf-0, intra
10.12.31.0/31, ubest/mbest: 1/0
    *via 10.12.21.1, Eth1/1, [110/80], 01:10:56, ospf-0, intra
10.12.32.0/31, ubest/mbest: 1/0
    *via 10.12.22.1, Eth1/2, [110/80], 01:16:40, ospf-0, intra

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
 10.10.1.0         1 FULL/ -          02:07:22 10.12.31.1      Eth1/1
 10.10.2.0         1 FULL/ -          01:27:47 10.12.32.1      Eth1/2
Leaf_3# show ip route ospf-0
IP Route Table for VRF "default"
'*' denotes best ucast next-hop
'**' denotes best mcast next-hop
'[x/y]' denotes [preference/metric]
'%<string>' in via output denotes VRF <string>

10.10.1.0/32, ubest/mbest: 1/0
    *via 10.12.31.1, Eth1/1, [110/41], 02:07:22, ospf-0, intra
10.10.2.0/32, ubest/mbest: 1/0
    *via 10.12.32.1, Eth1/2, [110/41], 01:27:52, ospf-0, intra
10.10.10.0/32, ubest/mbest: 2/0
    *via 10.12.31.1, Eth1/1, [110/81], 02:07:22, ospf-0, intra
    *via 10.12.32.1, Eth1/2, [110/81], 01:27:52, ospf-0, intra
10.10.20.0/32, ubest/mbest: 2/0
    *via 10.12.31.1, Eth1/1, [110/81], 01:13:04, ospf-0, intra
    *via 10.12.32.1, Eth1/2, [110/81], 01:18:48, ospf-0, intra
10.12.11.0/31, ubest/mbest: 1/0
    *via 10.12.31.1, Eth1/1, [110/80], 02:07:22, ospf-0, intra
10.12.12.0/31, ubest/mbest: 1/0
    *via 10.12.32.1, Eth1/2, [110/80], 01:27:52, ospf-0, intra
10.12.21.0/31, ubest/mbest: 1/0
    *via 10.12.31.1, Eth1/1, [110/80], 01:13:49, ospf-0, intra
10.12.22.0/31, ubest/mbest: 1/0
    *via 10.12.32.1, Eth1/2, [110/80], 01:27:52, ospf-0, intra

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
 10.10.10.0        1 FULL/ -          02:11:00 10.12.11.0      Eth1/1
 10.10.20.0        1 FULL/ -          01:13:56 10.12.21.0      Eth1/2
 10.10.30.0        1 FULL/ -          02:08:19 10.12.31.0      Eth1/3
Spine_1# show ip route ospf-0
IP Route Table for VRF "default"
'*' denotes best ucast next-hop
'**' denotes best mcast next-hop
'[x/y]' denotes [preference/metric]
'%<string>' in via output denotes VRF <string>

10.10.2.0/32, ubest/mbest: 3/0
    *via 10.12.11.0, Eth1/1, [110/81], 01:29:40, ospf-0, intra
    *via 10.12.21.0, Eth1/2, [110/81], 01:14:02, ospf-0, intra
    *via 10.12.31.0, Eth1/3, [110/81], 01:28:50, ospf-0, intra
10.10.10.0/32, ubest/mbest: 1/0
    *via 10.12.11.0, Eth1/1, [110/41], 02:11:00, ospf-0, intra
10.10.20.0/32, ubest/mbest: 1/0
    *via 10.12.21.0, Eth1/2, [110/41], 01:14:02, ospf-0, intra
10.10.30.0/32, ubest/mbest: 1/0
    *via 10.12.31.0, Eth1/3, [110/41], 02:08:20, ospf-0, intra
10.12.12.0/31, ubest/mbest: 1/0
    *via 10.12.11.0, Eth1/1, [110/80], 02:11:00, ospf-0, intra
10.12.22.0/31, ubest/mbest: 1/0
    *via 10.12.21.0, Eth1/2, [110/80], 01:14:02, ospf-0, intra
10.12.32.0/31, ubest/mbest: 1/0
    *via 10.12.31.0, Eth1/3, [110/80], 02:08:20, ospf-0, intra

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
 10.10.10.0        1 FULL/ -          01:31:00 10.12.12.0      Eth1/1
 10.10.20.0        1 FULL/ -          01:21:06 10.12.22.0      Eth1/2
 10.10.30.0        1 FULL/ -          01:30:05 10.12.32.0      Eth1/3
Spine_2# show ip route ospf-0
IP Route Table for VRF "default"
'*' denotes best ucast next-hop
'**' denotes best mcast next-hop
'[x/y]' denotes [preference/metric]
'%<string>' in via output denotes VRF <string>

10.10.1.0/32, ubest/mbest: 3/0
    *via 10.12.12.0, Eth1/1, [110/81], 01:31:00, ospf-0, intra
    *via 10.12.22.0, Eth1/2, [110/81], 01:15:22, ospf-0, intra
    *via 10.12.32.0, Eth1/3, [110/81], 01:30:10, ospf-0, intra
10.10.10.0/32, ubest/mbest: 1/0
    *via 10.12.12.0, Eth1/1, [110/41], 01:31:00, ospf-0, intra
10.10.20.0/32, ubest/mbest: 1/0
    *via 10.12.22.0, Eth1/2, [110/41], 01:21:06, ospf-0, intra
10.10.30.0/32, ubest/mbest: 1/0
    *via 10.12.32.0, Eth1/3, [110/41], 01:30:10, ospf-0, intra
10.12.11.0/31, ubest/mbest: 1/0
    *via 10.12.12.0, Eth1/1, [110/80], 01:31:00, ospf-0, intra
10.12.21.0/31, ubest/mbest: 1/0
    *via 10.12.22.0, Eth1/2, [110/80], 01:21:06, ospf-0, intra
10.12.31.0/31, ubest/mbest: 1/0
    *via 10.12.32.0, Eth1/3, [110/80], 01:30:10, ospf-0, intra

Spine_2#
```
</details>