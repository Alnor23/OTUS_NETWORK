# EIGRP 
______  
## Задание:  
1. В офисе С.-Петербург настроить EIGRP.
2. R32 получает только маршрут по умолчанию.
3. R16-17 анонсируют только суммарные префиксы.
4. Использовать EIGRP named-mode для настройки сети.  
Настройка осуществляется одновременно для IPv4 и IPv6
### Схема  
![scheme](https://github.com/Alnor23/OTUS_NETWORK/blob/main/labs/lab8_eigrp/screenshots/scheme.png) 
### Таблица адресации  
Management
Device | IPv4 Address  | Subnet Mask    | IPv6 GLA Address         | IPv6 LLA Address
-------|---------------|----------------|--------------------------|------------------
R18    |10.2.1.2       |255.255.255.240 |20FF:0DB8:ACAD:2001::18/64|FE80::18/8
R17    |10.2.1.3       |255.255.255.240 |20FF:0DB8:ACAD:2001::17/64|FE80::17/8
R16    |10.2.1.4       |255.255.255.240 |20FF:0DB8:ACAD:2001::16/64|FE80::16/8
R32    |10.2.1.5       |255.255.255.240 |20FF:0DB8:ACAD:2001::32/64|FE80::32/8
SW9    |10.2.1.18      |255.255.255.248 |n/a                       |n/a
SW10   |10.2.1.19      |255.255.255.248 |n/a                       |n/a

Link
Device | Port | Address type | Address                      | Network
------ |------|--------------|------------------------------|---------
R17    |e0/1  |IPv4          |10.2.2.1/30                   |10.2.2.0/30
R17    |e0/1  |IPv6          |20FF:0DB8:ACAD:2011::17:1/64  |20FF:0DB8:ACAD:2011::/64
R18    |e0/1  |IPv4          |10.2.2.2/30                   |10.2.2.0/30
R18    |e0/1  |IPv6          |20FF:0DB8:ACAD:2011::18:1/64  |20FF:0DB8:ACAD:2011::/64
||||
R18    |e0/0  |IPv4          |10.2.2.5/30                   |10.2.2.4/30
R18    |e0/0  |IPv6          |20FF:0DB8:ACAD:2012::18:/64   |20FF:0DB8:ACAD:2012::/64
R16    |e0/1  |IPv4          |10.2.2.6/30                   |10.2.2.4/30
R16    |e0/1  |IPv6          |20FF:0DB8:ACAD:2012::16:1/64  |20FF:0DB8:ACAD:2012::/64
||||
R16    |e0/3  |IPv4          |10.2.2.9/30                   |10.2.2.8/30
R16    |e0/3  |IPv6          |20FF:0DB8:ACAD:2013::16:3/64  |20FF:0DB8:ACAD:2013::/64
R32    |e0/0  |IPv4          |10.2.2.10/30                  |10.2.2.8/30
R32    |e0/0  |IPv6          |20FF:0DB8:ACAD:2013::32:/64   |20FF:0DB8:ACAD:2013::/64
### Часть 1. В офисе С.-Петербург настроить EIGRP.  
Для выполнения данного задания необходимо включить на маршрутизаторах named-mode EIGRP:
```
R18(config)#router eigrp SPB
```
Далее необходимо задать сети IPv4 для EIGRP (Так как все интерфейсы в локации СПБ находятся в сети 10.2.0.0/16) следовательно еоманда network будет следующая:
```
R18(config-router-af)#network 10.2.0.0 0.0.255.255
```
Такжe задаем router-id для каждого маршрутизатора для IPv4 и IPv6:
```
R18(config-router-af)#eigrp router-id 1.1.1.1
```
Аналогично настраивается на остальных маршрутизаторах.  
### Часть 2. R32 получает только маршрут по умолчанию.
Для выполнения этой части необходимо:
Обьявить маршрут по умолчанию для IPv4 и IPv6 на пограничном маршрутизаторе R18:  
```
R18(config)#ip route 0.0.0.0 0.0.0.0 50.50.1.9
R18(config)#ipv6 route ::/0 20FF:0DB8:ACAD:7003::24:3
```
И включить редистрибьюцию:  
```
address-family ipv4 unicast autonomous-system 1
topology base
redistribute static

address-family ipv4 unicast autonomous-system 1
topology base
redistribute static
```
Проверка получения маршрута по умолчанию намаршрутизаторе R32:  
```
R32#sh ip route eigrp
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is 10.2.2.9 to network 0.0.0.0

D*EX  0.0.0.0/0 [170/2048000] via 10.2.2.9, 00:00:49, Ethernet0/0
      10.0.0.0/8 is variably subnetted, 8 subnets, 4 masks
D        10.2.1.16/29 [90/2560000] via 10.2.2.9, 01:20:56, Ethernet0/0
D        10.2.2.0/30 [90/2048000] via 10.2.2.9, 01:20:56, Ethernet0/0
D        10.2.2.4/30 [90/1536000] via 10.2.2.9, 01:20:56, Ethernet0/0
D        10.2.3.0/28 [90/2560000] via 10.2.2.9, 01:20:56, Ethernet0/0
R32#sh ipv6 route eigrp
IPv6 Routing Table - default - 10 entries
Codes: C - Connected, L - Local, S - Static, U - Per-user Static route
       B - BGP, HA - Home Agent, MR - Mobile Router, R - RIP
       H - NHRP, I1 - ISIS L1, I2 - ISIS L2, IA - ISIS interarea
       IS - ISIS summary, D - EIGRP, EX - EIGRP external, NM - NEMO
       ND - ND Default, NDp - ND Prefix, DCE - Destination, NDr - Redirect
       O - OSPF Intra, OI - OSPF Inter, OE1 - OSPF ext 1, OE2 - OSPF ext 2
       ON1 - OSPF NSSA ext 1, ON2 - OSPF NSSA ext 2, la - LISP alt
       lr - LISP site-registrations, ld - LISP dyn-eid, a - Application
EX  ::/0 [170/2048000]
     via FE80::16, Ethernet0/0
D   20FF:DB8:ACAD:2011::/64 [90/2048000]
     via FE80::16, Ethernet0/0
D   20FF:DB8:ACAD:2012::/64 [90/1536000]
     via FE80::16, Ethernet0/0
D   20FF:DB8:ACAD:7003::/64 [90/2048000]
     via FE80::16, Ethernet0/0
D   20FF:DB8:ACAD:7004::/64 [90/2048000]
     via FE80::16, Ethernet0/0
```
### Часть 3. R16-17 анонсируют только суммарные префиксы.  
Для выполнения этой части необходимо включить auto-summary на R16-17:  
```
 address-family ipv4 unicast autonomous-system 1
  !
  topology base
   auto-summary
```
Проверка:  
```
R18#show ip eigrp topology all-links
EIGRP-IPv4 VR(SPB) Topology Table for AS(1)/ID(1.1.1.1)
Codes: P - Passive, A - Active, U - Update, Q - Query, R - Reply,
       r - reply Status, s - sia Status

P 10.2.2.8/30, 1 successors, FD is 196608000, serno 10
        via 10.2.2.6 (196608000/131072000), Ethernet0/0
P 10.2.2.0/30, 1 successors, FD is 131072000, serno 2
        via Connected, Ethernet0/1
P 0.0.0.0/0, 1 successors, FD is 131072000, serno 11
        via Rstatic (131072000/0)
P 10.2.1.16/29, 1 successors, FD is 196608000, serno 6
        via 10.2.2.1 (196608000/131072000), Ethernet0/1
P 10.2.2.4/30, 1 successors, FD is 131072000, serno 1
        via Connected, Ethernet0/0
P 10.2.1.0/28, 1 successors, FD is 163840, serno 3
        via Connected, Loopback1
        via 10.2.2.6 (131153920/163840), Ethernet0/0
        via 10.2.2.1 (131153920/163840), Ethernet0/1
P 10.2.3.0/28, 1 successors, FD is 196608000, serno 7
        via 10.2.2.1 (196608000/131072000), Ethernet0/1
```
Здесь видно что маршрутизатор R18 получил от маршрутизаторов R16-17 обьединенные маршруты.    
 _______
  - [Конфигурации устройств](https://github.com/Alnor23/OTUS_NETWORK/tree/main/labs/lab8_eigrp/config)
  





