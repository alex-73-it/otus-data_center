# Адресное пространство и схема сети
![](<Схема сети.png>)
# Конфигурации устройств
<details>
<summary> Конфиг Leaf_1 </summary>

```
key chain ospf
  key 0
    key-string 7 0700325c48
    cryptographic-algorithm HMAC-SHA-256

feature isis

router isis 0
  net 49.0001.0100.1001.0000.00
  is-type level-1
  log-adjacency-changes
  authentication-type md5 level-1
  authentication key-chain ospf level-1
  address-family ipv4 unicast
  passive-interface default level-1-2

interface Vlan11
  isis metric 1 level-1
  isis metric 1 level-2
  isis network point-to-point
  ip router isis 0

interface loopback0
  ip router isis 0

interface Ethernet1/1
  isis network point-to-point
  isis authentication-type md5
  isis authentication key-chain ospf
  ip router isis 0
  no isis passive-interface level-1-2

interface Ethernet1/2
  isis network point-to-point
  isis authentication-type md5
  isis authentication key-chain ospf
  ip router isis 0
  no isis passive-interface level-1-2
```
</details>

<details>
<summary> Конфиг Leaf_2 </summary>

```
key chain ospf
  key 0
    key-string 7 0700325c48
    cryptographic-algorithm HMAC-SHA-256

feature isis

router isis 0
  net 49.0001.0100.1002.0000.00
  is-type level-1
  log-adjacency-changes
  authentication-type md5 level-1
  authentication key-chain ospf level-1
  address-family ipv4 unicast
  passive-interface default level-1-2

interface Vlan22
  isis metric 1 level-1
  isis metric 1 level-2
  isis network point-to-point
  ip router isis 0

interface loopback0
  ip router isis 0

interface Ethernet1/1
  isis network point-to-point
  isis authentication-type md5
  isis authentication key-chain ospf
  ip router isis 0
  no isis passive-interface level-1-2

interface Ethernet1/2
  isis network point-to-point
  isis authentication-type md5
  isis authentication key-chain ospf
  ip router isis 0
  no isis passive-interface level-1-2
```
</details>

<details>
<summary> Конфиг Leaf_3 </summary>

```
key chain ospf
  key 0
    key-string 7 0700325c48
    cryptographic-algorithm HMAC-SHA-256

feature isis

router isis 0
  net 49.0001.0100.1003.0000.00
  is-type level-1
  log-adjacency-changes
  authentication-type md5 level-1
  authentication key-chain ospf level-1
  address-family ipv4 unicast
  passive-interface default level-1-2

interface Vlan33
  isis metric 1 level-1
  isis metric 1 level-2
  isis network point-to-point
  ip router isis 0

interface Vlan34
  isis metric 1 level-1
  isis metric 1 level-2
  isis network point-to-point
  ip router isis 0

interface loopback0
  ip router isis 0

interface Ethernet1/1
  isis network point-to-point
  isis authentication-type md5
  isis authentication key-chain ospf
  ip router isis 0
  no isis passive-interface level-1-2

interface Ethernet1/2
  isis network point-to-point
  isis authentication-type md5
  isis authentication key-chain ospf
  ip router isis 0
  no isis passive-interface level-1-2
```
</details>

<details>
<summary> Конфиг Spine_1 </summary>

```
key chain ospf
  key 0
    key-string 7 0700325c48
    cryptographic-algorithm HMAC-SHA-256

feature isis

router isis 0
  net 49.0001.0100.1000.1000.00
  is-type level-1
  log-adjacency-changes
  authentication-type md5 level-1
  authentication key-chain ospf level-1
  address-family ipv4 unicast
  passive-interface default level-1-2

interface loopback0
  ip router isis 0

interface Ethernet1/1
  isis network point-to-point
  isis authentication-type md5
  isis authentication key-chain ospf
  ip router isis 0
  no isis passive-interface level-1-2

interface Ethernet1/2
  isis network point-to-point
  isis authentication-type md5
  isis authentication key-chain ospf
  ip router isis 0
  no isis passive-interface level-1-2

interface Ethernet1/3
  isis network point-to-point
  isis authentication-type md5
  isis authentication key-chain ospf
  ip router isis 0
  no isis passive-interface level-1-2
```
</details>

<details>
<summary> Конфиг Spine_2 </summary>

```
key chain ospf
  key 0
    key-string 7 0700325c48
    cryptographic-algorithm HMAC-SHA-256

feature isis

router isis 0
  net 49.0001.0100.1000.2000.00
  is-type level-1
  log-adjacency-changes
  authentication-type md5 level-1
  authentication key-chain ospf level-1
  address-family ipv4 unicast
  passive-interface default level-1-2

interface loopback0
  ip router isis 0

interface Ethernet1/1
  isis network point-to-point
  isis authentication-type md5
  isis authentication key-chain ospf
  ip router isis 0
  no isis passive-interface level-1-2

interface Ethernet1/2
  isis network point-to-point
  isis authentication-type md5
  isis authentication key-chain ospf
  ip router isis 0
  no isis passive-interface level-1-2

interface Ethernet1/3
  isis network point-to-point
  isis authentication-type md5
  isis authentication key-chain ospf
  ip router isis 0
  no isis passive-interface level-1-2
```
</details>

# Проверка работоспособности
1. Вывод команд show на устройствах
<details> 
<summary> Leaf_1 </summary>

```
Leaf_1# sh isis adjacency
IS-IS process: 0 VRF: default
IS-IS adjacency database:
Legend: '!': No AF level connectivity in given topology
System ID       SNPA            Level  State  Hold Time  Interface
Spine_1         N/A             1      UP     00:00:31   Ethernet1/1
Spine_2         N/A             1      UP     00:00:26   Ethernet1/2

Leaf_1# show isis database
IS-IS Process: 0 LSP database VRF: default
IS-IS Level-1 Link State Database
  LSPID                 Seq Number   Checksum  Lifetime   A/P/O/T
  Spine_1.00-00         0x000001D3   0xFCBD    1021       0/0/0/1
  Spine_2.00-00         0x000001CE   0x19ED    977        0/0/0/1
  Leaf_1.00-00        * 0x000001F8   0x9740    833        0/0/0/1
  Leaf_2.00-00          0x000001D1   0x3CF8    1006       0/0/0/1
  Leaf_3.00-00          0x000001D3   0x2BA1    993        0/0/0/1

IS-IS Level-2 Link State Database
  LSPID                 Seq Number   Checksum  Lifetime   A/P/O/T

Leaf_1# show ip route isis-0
IP Route Table for VRF "default"
'*' denotes best ucast next-hop
'**' denotes best mcast next-hop
'[x/y]' denotes [preference/metric]
'%<string>' in via output denotes VRF <string>

10.10.1.0/32, ubest/mbest: 1/0
    *via 10.12.11.1, Eth1/1, [115/41], 00:27:49, isis-0, L1
10.10.2.0/32, ubest/mbest: 1/0
    *via 10.12.12.1, Eth1/2, [115/41], 00:27:16, isis-0, L1
10.10.20.0/32, ubest/mbest: 2/0
    *via 10.12.11.1, Eth1/1, [115/81], 00:27:49, isis-0, L1
    *via 10.12.12.1, Eth1/2, [115/81], 00:27:16, isis-0, L1
10.10.30.0/32, ubest/mbest: 2/0
    *via 10.12.11.1, Eth1/1, [115/81], 00:27:49, isis-0, L1
    *via 10.12.12.1, Eth1/2, [115/81], 00:27:16, isis-0, L1
10.12.21.0/31, ubest/mbest: 1/0
    *via 10.12.11.1, Eth1/1, [115/80], 00:27:49, isis-0, L1
10.12.22.0/31, ubest/mbest: 1/0
    *via 10.12.12.1, Eth1/2, [115/80], 00:27:16, isis-0, L1
10.12.31.0/31, ubest/mbest: 1/0
    *via 10.12.11.1, Eth1/1, [115/80], 00:27:49, isis-0, L1
10.12.32.0/31, ubest/mbest: 1/0
    *via 10.12.12.1, Eth1/2, [115/80], 00:27:16, isis-0, L1
10.13.22.0/24, ubest/mbest: 2/0
    *via 10.12.11.1, Eth1/1, [115/81], 00:27:49, isis-0, L1
    *via 10.12.12.1, Eth1/2, [115/81], 00:27:16, isis-0, L1
10.13.33.0/24, ubest/mbest: 2/0
    *via 10.12.11.1, Eth1/1, [115/81], 00:27:49, isis-0, L1
    *via 10.12.12.1, Eth1/2, [115/81], 00:27:16, isis-0, L1
10.13.34.0/24, ubest/mbest: 2/0
    *via 10.12.11.1, Eth1/1, [115/81], 00:27:49, isis-0, L1
    *via 10.12.12.1, Eth1/2, [115/81], 00:27:16, isis-0, L1
```
</details>

<details> 
<summary> Leaf_2 </summary>

```
Leaf_2# show isis adjacency
IS-IS process: 0 VRF: default
IS-IS adjacency database:
Legend: '!': No AF level connectivity in given topology
System ID       SNPA            Level  State  Hold Time  Interface
Spine_1         N/A             1      UP     00:00:30   Ethernet1/1
Spine_2         N/A             1      UP     00:00:23   Ethernet1/2

Leaf_2# show isis database
IS-IS Process: 0 LSP database VRF: default
IS-IS Level-1 Link State Database
  LSPID                 Seq Number   Checksum  Lifetime   A/P/O/T
  Spine_1.00-00         0x000001D3   0xFCBD    931        0/0/0/1
  Spine_2.00-00         0x000001CE   0x19ED    887        0/0/0/1
  Leaf_1.00-00          0x000001F8   0x9740    741        0/0/0/1
  Leaf_2.00-00        * 0x000001D1   0x3CF8    918        0/0/0/1
  Leaf_3.00-00          0x000001D3   0x2BA1    904        0/0/0/1

IS-IS Level-2 Link State Database
  LSPID                 Seq Number   Checksum  Lifetime   A/P/O/T

Leaf_2# show ip route isis-0
IP Route Table for VRF "default"
'*' denotes best ucast next-hop
'**' denotes best mcast next-hop
'[x/y]' denotes [preference/metric]
'%<string>' in via output denotes VRF <string>

10.10.1.0/32, ubest/mbest: 1/0
    *via 10.12.21.1, Eth1/1, [115/41], 2d22h, isis-0, L1
10.10.2.0/32, ubest/mbest: 1/0
    *via 10.12.22.1, Eth1/2, [115/41], 2d21h, isis-0, L1
10.10.10.0/32, ubest/mbest: 2/0
    *via 10.12.21.1, Eth1/1, [115/81], 00:29:19, isis-0, L1
    *via 10.12.22.1, Eth1/2, [115/81], 00:28:46, isis-0, L1
10.10.30.0/32, ubest/mbest: 2/0
    *via 10.12.21.1, Eth1/1, [115/81], 2d22h, isis-0, L1
    *via 10.12.22.1, Eth1/2, [115/81], 2d21h, isis-0, L1
10.12.11.0/31, ubest/mbest: 1/0
    *via 10.12.21.1, Eth1/1, [115/80], 2d22h, isis-0, L1
10.12.12.0/31, ubest/mbest: 1/0
    *via 10.12.22.1, Eth1/2, [115/80], 2d21h, isis-0, L1
10.12.31.0/31, ubest/mbest: 1/0
    *via 10.12.21.1, Eth1/1, [115/80], 2d22h, isis-0, L1
10.12.32.0/31, ubest/mbest: 1/0
    *via 10.12.22.1, Eth1/2, [115/80], 2d21h, isis-0, L1
10.13.11.0/24, ubest/mbest: 2/0
    *via 10.12.21.1, Eth1/1, [115/81], 00:29:19, isis-0, L1
    *via 10.12.22.1, Eth1/2, [115/81], 00:28:46, isis-0, L1
10.13.33.0/24, ubest/mbest: 2/0
    *via 10.12.21.1, Eth1/1, [115/81], 2d22h, isis-0, L1
    *via 10.12.22.1, Eth1/2, [115/81], 2d21h, isis-0, L1
10.13.34.0/24, ubest/mbest: 2/0
    *via 10.12.21.1, Eth1/1, [115/81], 2d22h, isis-0, L1
    *via 10.12.22.1, Eth1/2, [115/81], 2d21h, isis-0, L1
```
</details>

<details> 
<summary> Leaf_3 </summary>

```
Leaf_3# show isis adjacency
IS-IS process: 0 VRF: default
IS-IS adjacency database:
Legend: '!': No AF level connectivity in given topology
System ID       SNPA            Level  State  Hold Time  Interface
Spine_1         N/A             1      UP     00:00:27   Ethernet1/1
Spine_2         N/A             1      UP     00:00:26   Ethernet1/2

Leaf_3# show isis database
IS-IS Process: 0 LSP database VRF: default
IS-IS Level-1 Link State Database
  LSPID                 Seq Number   Checksum  Lifetime   A/P/O/T
  Spine_1.00-00         0x000001D3   0xFCBD    880        0/0/0/1
  Spine_2.00-00         0x000001CE   0x19ED    836        0/0/0/1
  Leaf_1.00-00          0x000001F8   0x9740    690        0/0/0/1
  Leaf_2.00-00          0x000001D1   0x3CF8    864        0/0/0/1
  Leaf_3.00-00        * 0x000001D3   0x2BA1    854        0/0/0/1

IS-IS Level-2 Link State Database
  LSPID                 Seq Number   Checksum  Lifetime   A/P/O/T

Leaf_3# show ip route isis-0
IP Route Table for VRF "default"
'*' denotes best ucast next-hop
'**' denotes best mcast next-hop
'[x/y]' denotes [preference/metric]
'%<string>' in via output denotes VRF <string>

10.10.1.0/32, ubest/mbest: 1/0
    *via 10.12.31.1, Eth1/1, [115/41], 2d22h, isis-0, L1
10.10.2.0/32, ubest/mbest: 1/0
    *via 10.12.32.1, Eth1/2, [115/41], 2d21h, isis-0, L1
10.10.10.0/32, ubest/mbest: 2/0
    *via 10.12.31.1, Eth1/1, [115/81], 00:30:09, isis-0, L1
    *via 10.12.32.1, Eth1/2, [115/81], 00:29:36, isis-0, L1
10.10.20.0/32, ubest/mbest: 2/0
    *via 10.12.31.1, Eth1/1, [115/81], 2d22h, isis-0, L1
    *via 10.12.32.1, Eth1/2, [115/81], 2d21h, isis-0, L1
10.12.11.0/31, ubest/mbest: 1/0
    *via 10.12.31.1, Eth1/1, [115/80], 2d22h, isis-0, L1
10.12.12.0/31, ubest/mbest: 1/0
    *via 10.12.32.1, Eth1/2, [115/80], 2d21h, isis-0, L1
10.12.21.0/31, ubest/mbest: 1/0
    *via 10.12.31.1, Eth1/1, [115/80], 2d22h, isis-0, L1
10.12.22.0/31, ubest/mbest: 1/0
    *via 10.12.32.1, Eth1/2, [115/80], 2d21h, isis-0, L1
10.13.11.0/24, ubest/mbest: 2/0
    *via 10.12.31.1, Eth1/1, [115/81], 00:30:09, isis-0, L1
    *via 10.12.32.1, Eth1/2, [115/81], 00:29:36, isis-0, L1
10.13.22.0/24, ubest/mbest: 2/0
    *via 10.12.31.1, Eth1/1, [115/81], 2d22h, isis-0, L1
    *via 10.12.32.1, Eth1/2, [115/81], 2d21h, isis-0, L1
```
</details>

<details> 
<summary> Spine_1 </summary>

```
Spine_1# show isis adjacency
IS-IS process: 0 VRF: default
IS-IS adjacency database:
Legend: '!': No AF level connectivity in given topology
System ID       SNPA            Level  State  Hold Time  Interface
Leaf_1          N/A             1      UP     00:00:22   Ethernet1/1
Leaf_2          N/A             1      UP     00:00:26   Ethernet1/2
Leaf_3          N/A             1      UP     00:00:30   Ethernet1/3

Spine_1# sh isis database
IS-IS Process: 0 LSP database VRF: default
IS-IS Level-1 Link State Database
  LSPID                 Seq Number   Checksum  Lifetime   A/P/O/T
  Spine_1.00-00       * 0x000001D3   0xFCBD    823        0/0/0/1
  Spine_2.00-00         0x000001CE   0x19ED    777        0/0/0/1
  Leaf_1.00-00          0x000001F8   0x9740    633        0/0/0/1
  Leaf_2.00-00          0x000001D1   0x3CF8    808        0/0/0/1
  Leaf_3.00-00          0x000001D3   0x2BA1    796        0/0/0/1

IS-IS Level-2 Link State Database
  LSPID                 Seq Number   Checksum  Lifetime   A/P/O/T

Spine_1# show ip route isis-0
IP Route Table for VRF "default"
'*' denotes best ucast next-hop
'**' denotes best mcast next-hop
'[x/y]' denotes [preference/metric]
'%<string>' in via output denotes VRF <string>

10.10.2.0/32, ubest/mbest: 3/0
    *via 10.12.11.0, Eth1/1, [115/81], 00:30:35, isis-0, L1
    *via 10.12.21.0, Eth1/2, [115/81], 2d21h, isis-0, L1
    *via 10.12.31.0, Eth1/3, [115/81], 2d21h, isis-0, L1
10.10.10.0/32, ubest/mbest: 1/0
    *via 10.12.11.0, Eth1/1, [115/41], 00:31:07, isis-0, L1
10.10.20.0/32, ubest/mbest: 1/0
    *via 10.12.21.0, Eth1/2, [115/41], 2d22h, isis-0, L1
10.10.30.0/32, ubest/mbest: 1/0
    *via 10.12.31.0, Eth1/3, [115/41], 2d22h, isis-0, L1
10.12.12.0/31, ubest/mbest: 1/0
    *via 10.12.11.0, Eth1/1, [115/80], 00:31:07, isis-0, L1
10.12.22.0/31, ubest/mbest: 1/0
    *via 10.12.21.0, Eth1/2, [115/80], 2d22h, isis-0, L1
10.12.32.0/31, ubest/mbest: 1/0
    *via 10.12.31.0, Eth1/3, [115/80], 2d22h, isis-0, L1
10.13.11.0/24, ubest/mbest: 1/0
    *via 10.12.11.0, Eth1/1, [115/41], 00:31:07, isis-0, L1
10.13.22.0/24, ubest/mbest: 1/0
    *via 10.12.21.0, Eth1/2, [115/41], 2d22h, isis-0, L1
10.13.33.0/24, ubest/mbest: 1/0
    *via 10.12.31.0, Eth1/3, [115/41], 2d22h, isis-0, L1
10.13.34.0/24, ubest/mbest: 1/0
    *via 10.12.31.0, Eth1/3, [115/41], 2d22h, isis-0, L1
```
</details>

<details> 
<summary> Spine_2 </summary>

```
Spine_2# show isis adjacency
IS-IS process: 0 VRF: default
IS-IS adjacency database:
Legend: '!': No AF level connectivity in given topology
System ID       SNPA            Level  State  Hold Time  Interface
Leaf_1          N/A             1      UP     00:00:25   Ethernet1/1
Leaf_2          N/A             1      UP     00:00:25   Ethernet1/2
Leaf_3          N/A             1      UP     00:00:28   Ethernet1/3

Spine_2# show isis database
IS-IS Process: 0 LSP database VRF: default
IS-IS Level-1 Link State Database
  LSPID                 Seq Number   Checksum  Lifetime   A/P/O/T
  Spine_1.00-00         0x000001D3   0xFCBD    709        0/0/0/1
  Spine_2.00-00       * 0x000001CE   0x19ED    667        0/0/0/1
  Leaf_1.00-00          0x000001F9   0xDE08    1109       0/0/0/1
  Leaf_2.00-00          0x000001D1   0x3CF8    696        0/0/0/1
  Leaf_3.00-00          0x000001D3   0x2BA1    683        0/0/0/1

IS-IS Level-2 Link State Database
  LSPID                 Seq Number   Checksum  Lifetime   A/P/O/T

Spine_2# show ip route isis-0
IP Route Table for VRF "default"
'*' denotes best ucast next-hop
'**' denotes best mcast next-hop
'[x/y]' denotes [preference/metric]
'%<string>' in via output denotes VRF <string>

10.10.1.0/32, ubest/mbest: 3/0
    *via 10.12.12.0, Eth1/1, [115/81], 00:32:27, isis-0, L1
    *via 10.12.22.0, Eth1/2, [115/81], 2d21h, isis-0, L1
    *via 10.12.32.0, Eth1/3, [115/81], 2d21h, isis-0, L1
10.10.10.0/32, ubest/mbest: 1/0
    *via 10.12.12.0, Eth1/1, [115/41], 00:32:27, isis-0, L1
10.10.20.0/32, ubest/mbest: 1/0
    *via 10.12.22.0, Eth1/2, [115/41], 2d21h, isis-0, L1
10.10.30.0/32, ubest/mbest: 1/0
    *via 10.12.32.0, Eth1/3, [115/41], 2d21h, isis-0, L1
10.12.11.0/31, ubest/mbest: 1/0
    *via 10.12.12.0, Eth1/1, [115/80], 00:32:27, isis-0, L1
10.12.21.0/31, ubest/mbest: 1/0
    *via 10.12.22.0, Eth1/2, [115/80], 2d21h, isis-0, L1
10.12.31.0/31, ubest/mbest: 1/0
    *via 10.12.32.0, Eth1/3, [115/80], 2d21h, isis-0, L1
10.13.11.0/24, ubest/mbest: 1/0
    *via 10.12.12.0, Eth1/1, [115/41], 00:32:27, isis-0, L1
10.13.22.0/24, ubest/mbest: 1/0
    *via 10.12.22.0, Eth1/2, [115/41], 2d21h, isis-0, L1
10.13.33.0/24, ubest/mbest: 1/0
    *via 10.12.32.0, Eth1/3, [115/41], 2d21h, isis-0, L1
10.13.34.0/24, ubest/mbest: 1/0
    *via 10.12.32.0, Eth1/3, [115/41], 2d21h, isis-0, L1
```
</details>

2. Проверка доступности сетей c помощью ping
<details>
<summary> ping с PC_1 до осатльных PC </summary>

```
pc_1> ping 10.13.22.2

84 bytes from 10.13.22.2 icmp_seq=1 ttl=61 time=15.356 ms
84 bytes from 10.13.22.2 icmp_seq=2 ttl=61 time=11.191 ms
84 bytes from 10.13.22.2 icmp_seq=3 ttl=61 time=12.566 ms
84 bytes from 10.13.22.2 icmp_seq=4 ttl=61 time=13.230 ms
^C
pc_1> ping 10.13.33.2

84 bytes from 10.13.33.2 icmp_seq=1 ttl=61 time=11.181 ms
84 bytes from 10.13.33.2 icmp_seq=2 ttl=61 time=13.286 ms
84 bytes from 10.13.33.2 icmp_seq=3 ttl=61 time=9.971 ms
84 bytes from 10.13.33.2 icmp_seq=4 ttl=61 time=9.887 ms
^C
pc_1> ping 10.13.34.2

84 bytes from 10.13.34.2 icmp_seq=1 ttl=61 time=8.932 ms
84 bytes from 10.13.34.2 icmp_seq=2 ttl=61 time=9.043 ms
84 bytes from 10.13.34.2 icmp_seq=3 ttl=61 time=8.097 ms
84 bytes from 10.13.34.2 icmp_seq=4 ttl=61 time=13.302 ms
^C
pc_1>
```
</detail>