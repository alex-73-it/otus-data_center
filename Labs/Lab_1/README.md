# Адресное пространство

ip = 10.Xx.Yy.z  
где  
- X = номер ЦОДа  
- x = 0 - loopback 0, 1 - loopback 1, 2 - p2p link, 3 - PC
- Y = номер leaf; 0 - когда loopback spine  
- y = номер spine или PC, если x = 3; 0 - когда loopback leaf
- z = номер попорядку 

# Схема с используемой адресацией:

![](<Схема сети.png>)

# Проверка доступности
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