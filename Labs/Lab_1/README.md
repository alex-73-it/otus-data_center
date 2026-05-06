# Адресное пространство

ip = 10.Xx.Yy.z  
где  
- X = номер ЦОДа  
- x = 0 - loopback 0, 1 - loopback 1, 2 - p2p link, 3 - PC
- Y = номер leaf; 0 - когда loopback spine  
- y = номер spine или PC, если x = 3; 0 - когда loopback leaf
- z = номер попорядку 

# Схема:

![](<Схема сети.png>)