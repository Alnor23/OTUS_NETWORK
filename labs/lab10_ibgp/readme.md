# iBGP  
______  
## Задание:  
1. Настройте iBGP в офисе Москва между маршрутизаторами R14 и R15.  
2. Настройте iBGP в провайдере Триада, с использованием RR.  
3. Настройте офис Москва так, чтобы приоритетным провайдером стал Ламас.  
4. Настройте офис С.-Петербург так, чтобы трафик до любого офиса распределялся по двум линкам одновременно.  
5. Все сети в лабораторной работе должны иметь IP связность.
### Схема  
![scheme](https://github.com/Alnor23/OTUS_NETWORK/blob/main/labs/lab9_bgp/screenshots/Scheme.png)  
### Таблица адресации
Management
Device | IPv4 Address  | Subnet Mask    | IPv6 GLA Address         | IPv6 LLA Address | AS
-------|---------------|----------------|--------------------------|----------------- | -------
R14    |10.1.1.3       |255.255.255.255 |20FF:0DB8:ACAD:1001::14/64|FE80::14/8        |1001(Москва)
R15    |10.1.1.5       |255.255.255.255 |20FF:0DB8:ACAD:1001::15/64|FE80::15/8        |1001(Москва)
R18    |10.2.1.2       |255.255.255.255 |20FF:0DB8:ACAD:2001::18/64|FE80::18/8        |1001(СПб)
R23    |10.10.1.2      |255.255.255.255 |20FF:0DB8:ACAD:4001::23/64|FE80::23/8        |520(Триада)
R24    |10.10.1.3      |255.255.255.255 |20FF:0DB8:ACAD:4001::24/64|FE80::24/8        |520(Триада)
R25    |10.10.1.4      |255.255.255.255 |20FF:0DB8:ACAD:4001::25/64|FE80::25/8        |520(Триада)
R26    |10.10.1.5      |255.255.255.255 |20FF:0DB8:ACAD:4001::26/64|FE80::26/8        |520(Триада)
R22    |10.11.1.2      |255.255.255.248 |20FF:ODB8:ACAD:5001::22/64|FE80::22/8        |101(Киторн)
R21    |10.12.1.2      |255.255.255.248 |20FF:0DB8:ACAD:6001::21/64|FE80::21/8        |301(Ламас)

Link outside AS
Device | Port | Address type | Address                      | Network
------ |------|--------------|------------------------------|---------
R23    |e0/0  |IPv4          |50.50.1.1/30                  |50.50.1.0/30
R23    |e0/0  |IPv6          |20FF:0DB8:ACAD:7001::23:/64   |20FF:0DB8:ACAD:7001::/64
R22    |e0/2  |IPv4          |50.50.1.2/30                  |50.50.1.0/30
R22    |e0/2  |IPv6          |20FF:0DB8:ACAD:7001::22:2/64   |20FF:0DB8:ACAD:7001::/64
||||
R24    |e0/0  |IPv4          |50.50.1.5/30                  |50.50.1.4/30
R24    |e0/0  |IPv6          |20FF:0DB8:ACAD:7002::24:/64   |20FF:0DB8:ACAD:7002::/64
R21    |e0/2  |IPv4          |50.50.1.6/30                  |50.50.1.4/30
R21    |e0/2  |IPv6          |20FF:0DB8:ACAD:7002::21:2/64  |20FF:0DB8:ACAD:7002::/64
||||
R24    |e0/3  |IPv4          |50.50.1.9/30                  |50.50.1.8/30
R24    |e0/3  |IPv6          |20FF:0DB8:ACAD:7003::24:3/64  |20FF:0DB8:ACAD:7003::/64
R18    |e0/2  |IPv4          |50.50.1.10/30                 |50.50.1.8/30
R18    |e0/2  |IPv6          |20FF:0DB8:ACAD:7003::18:2/64  |20FF:0DB8:ACAD:7003::/64
||||
R26    |e0/3  |IPv4          |50.50.1.13/30                 |50.50.1.12/30
R26    |e0/3  |IPv6          |20FF:0DB8:ACAD:7004::26:3/64  |20FF:0DB8:ACAD:7004::/64
R18    |e0/3  |IPv4          |50.50.1.14/30                 |50.50.1.12/30
R18    |e0/3  |IPv6          |20FF:0DB8:ACAD:7004::18:3/64  |20FF:0DB8:ACAD:7004::/64
||||
R26    |e0/1  |IPv4          |50.50.1.17/30                 |50.50.1.16/30
R26    |e0/1  |IPv6          |20FF:0DB8:ACAD:7005::26:1/64  |20FF:0DB8:ACAD:7005::/64
R28    |e0/0  |IPv4          |50.50.1.18/30                 |50.50.1.16/30
R28    |e0/0  |IPv6          |20FF:0DB8:ACAD:7005::28:0/64  |20FF:0DB8:ACAD:7005::/64
||||
R25    |e0/3  |IPv4          |50.50.1.21/30                 |50.50.1.20/30
R25    |e0/3  |IPv6          |20FF:0DB8:ACAD:7006::25:3/64  |20FF:0DB8:ACAD:7006::/64
R28    |e0/1  |IPv4          |50.50.1.22/30                 |50.50.1.20/30
R28    |e0/1  |IPv6          |20FF:0DB8:ACAD:7006::28:1/64  |20FF:0DB8:ACAD:7006::/64
||||
R25    |e0/1  |IPv4          |50.50.1.25/30                 |50.50.1.24/30
R25    |e0/1  |IPv6          |20FF:0DB8:ACAD:7007::25:1/64  |20FF:0DB8:ACAD:7007::/64
R27    |e0/0  |IPv4          |50.50.1.26/30                 |50.50.1.24/30
R27    |e0/0  |IPv6          |20FF:0DB8:ACAD:7007::27:/64   |20FF:0DB8:ACAD:7007::/64
||||
R22    |e0/0  |IPv4          |50.50.1.29/30                 |50.50.1.28/30
R22    |e0/0  |IPv6          |20FF:0DB8:ACAD:7008::22:/64   |20FF:0DB8:ACAD:7008::/64
R14    |e0/2  |IPv4          |50.50.1.30/30                 |50.50.1.28/30
R14    |e0/2  |IPv6          |20FF:0DB8:ACAD:7008::14:2/64  |20FF:0DB8:ACAD:7008::/64
||||
R22    |e0/1  |IPv4          |50.50.1.33/30                 |50.50.1.32/30
R22    |e0/1  |IPv6          |20FF:0DB8:ACAD:7009::22:1/64  |20FF:0DB8:ACAD:7009::/64
R21    |e0/1  |IPv4          |50.50.1.34/30                 |50.50.1.32/30
R21    |e0/1  |IPv6          |20FF:0DB8:ACAD:7009::21:1/64  |20FF:0DB8:ACAD:7009::/64
||||
R21    |e0/0  |IPv4          |50.50.1.37/30                 |50.50.1.36/30
R21    |e0/0  |IPv6          |20FF:0DB8:ACAD:7010::21:/64   |20FF:0DB8:ACAD:7010::/64
R15    |e0/2  |IPv4          |50.50.1.38/30                 |50.50.1.36/30
R15    |e0/2  |IPv6          |20FF:0DB8:ACAD:7010::15:2/64  |20FF:0DB8:ACAD:7010::/64

Link inside AS

Device | Port | Address type | Address                      | Network
------ |------|--------------|------------------------------|---------
R14    | e1/0 | IPv4         | 10.1.2.33/30                 | 10.1.2.32/30 
R14    | e1/0 | IPv6         | 20FF:0DB8:ACAD:1016::14:10/64| 20FF:0DB8:ACAD:1016::/64
R15    | e1/0 | IPv4         | 10.1.2.34/30                 | 10.1.2.32/30 
R15    | e1/0 | IPv6         | 20FF:0DB8:ACAD:1016::15:10/64| 20FF:0DB8:ACAD:1016::/64
||||
R23    |e0/1  |IPv4          |10.10.2.1/30                  |10.10.2.0/30
R23    |e0/1  |IPv6          |20FF:0DB8:ACAD:4011::23:1/64  |20FF:0DB8:ACAD:4011::/64
R25    |e0/0  |IPv4          |10.10.2.2/30                  |10.10.2.0/30
R25    |e0/0  |IPv6          |20FF:0DB8:ACAD:4011::25:/64   |20FF:0DB8:ACAD:4011::/64
||||
R25    |e0/2  |IPv4          |10.10.2.5/30                  |10.10.2.4/30
R25    |e0/2  |IPv6          |20FF:0DB8:ACAD:4012::25:2/64  |20FF:0DB8:ACAD:4012::/64
R26    |e0/2  |IPv4          |10.10.2.6/30                  |10.10.2.4/30
R26    |e0/2  |IPv6          |20FF:0DB8:ACAD:4012::26:2/64  |20FF:0DB8:ACAD:4000::/64
|||| 
R26    |e0/0  |IPv4          |10.10.2.9/30                  |10.10.2.8/30
R26    |e0/0  |IPv6          |20FF:0DB8:ACAD:4013::26:/64   |20FF:0DB8:ACAD:4013::/64
R24    |e0/1  |IPv4          |10.10.2.10/30                 |10.10.2.8/30
R24    |e0/1  |IPv6          |20FF:0DB8:ACAD:4013::24:1/64  |20FF:0DB8:ACAD:4013::/64
||||
R24    |e0/2  |IPv4          |10.10.2.13/30                 |10.10.2.12/30
R24    |e0/2  |IPv6          |20FF:0DB8:ACAD:4014::24:2/64  |20FF:0DB8:ACAD:4014::/64
R23    |e0/2  |IPv4          |10.10.2.14/30                 |10.10.2.12/30
R23    |e0/2  |IPv6          |20FF:0DB8:ACAD:4014::23:2/64  |20FF:0DB8:ACAD:4014::/64

### Часть 1. Настройте iBGP в офисе Москва между маршрутизаторами R14 и R15.
Для настройки соседства по iBGP между R14 и R15 необходимо добавить каждому маршрутизатору адрес loopback`а(сетевая доступность обеспечена OSPF) соответствующего соседа в конфигурацию, а также указать что обмен осуществляется между loopback интерфейсами:  
R14  
```
neighbor 10.1.1.5 remote-as 1001
neighbor 10.1.1.5 update-source l1
```
R15  
```
neighbor 10.1.1.3 remote-as 1001
neighbor 10.1.1.3 update-source l1
```
Выполним проверку на R14:  
```
R14(config-router)#do sh ip bgp sum
BGP router identifier 10.1.1.3, local AS number 1001
BGP table version is 6, main routing table version 6
3 network entries using 432 bytes of memory
4 path entries using 320 bytes of memory
4/3 BGP path/bestpath attribute entries using 608 bytes of memory
2 BGP AS-PATH entries using 64 bytes of memory
0 BGP route-map cache entries using 0 bytes of memory
0 BGP filter-list cache entries using 0 bytes of memory
BGP using 1424 total bytes of memory
BGP activity 3/0 prefixes, 4/0 paths, scan interval 60 secs

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
10.1.1.5        4         1001       5       8        6    0    0 00:00:30        1
50.50.1.29      4          101      12       8        5    0    0 00:03:20        1
```
Между R14 и R15 установилось iBGP соседство.

### Часть 2. Настройте iBGP в провайдере Триада, с использованием RR.  
Выполнение этой части заключается в настройке iBGP между маршрутизаторами R23, R24, R25, R26 с изпользованием Route-Reflector. Роль RR-server`а будет выполнять R24, а остальные RR-client.  
Создадим на R24 peer-group где в качестве соседей укажем R23, R25, R26(Также настроим обмен информацией между loopback интерфейсами):  
R24  
```
R24#sh run | s bgp
router bgp 520
 bgp router-id 10.10.1.3
 bgp log-neighbor-changes
 neighbor RR-CLIENT peer-group
 neighbor RR-CLIENT remote-as 520
 neighbor RR-CLIENT update-source Loopback1
 neighbor RR-CLIENT route-reflector-client
 neighbor 10.10.1.2 peer-group RR-CLIENT
 neighbor 10.10.1.4 peer-group RR-CLIENT
 neighbor 10.10.1.5 peer-group RR-CLIENT
 neighbor 50.50.1.6 remote-as 301
 neighbor 50.50.1.10 remote-as 2042
```
На остальных маршрутизаторах необходимо поднять соседство с R24 (Так как конфигурация на клиентах одинакова для примера берется R23):  
R23  
```
R23(config)#do sh run | s bgp
router bgp 520
 bgp log-neighbor-changes
 neighbor 10.10.1.3 remote-as 520
 neighbor 10.10.1.3 update-source Loopback1
```
Для проверки посмотрим вывод команды `show ip bgp summary` на RR-server R24 и RR-client R25:  
R24  
```
R24#sh ip bgp sum
BGP router identifier 10.10.1.3, local AS number 520
BGP table version is 2, main routing table version 2
1 network entries using 144 bytes of memory
1 path entries using 80 bytes of memory
1/1 BGP path/bestpath attribute entries using 152 bytes of memory
1 BGP AS-PATH entries using 24 bytes of memory
0 BGP route-map cache entries using 0 bytes of memory
0 BGP filter-list cache entries using 0 bytes of memory
BGP using 400 total bytes of memory
BGP activity 1/0 prefixes, 1/0 paths, scan interval 60 secs

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
10.10.1.2       4          520       9      10        2    0    0 00:04:54        0
10.10.1.4       4          520       6       9        2    0    0 00:02:55        0
10.10.1.5       4          520       5       9        2    0    0 00:02:54        0
50.50.1.6       4          301     164     163        2    0    0 02:25:10        0
50.50.1.10      4         2042     163     163        2    0    0 02:25:05        1
```
R25 
```
R25#sh ip bgp summary
BGP router identifier 10.10.1.4, local AS number 520
BGP table version is 1, main routing table version 1
1 network entries using 144 bytes of memory
1 path entries using 80 bytes of memory
1/0 BGP path/bestpath attribute entries using 152 bytes of memory
1 BGP AS-PATH entries using 24 bytes of memory
0 BGP route-map cache entries using 0 bytes of memory
0 BGP filter-list cache entries using 0 bytes of memory
BGP using 400 total bytes of memory
BGP activity 1/0 prefixes, 1/0 paths, scan interval 60 secs

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
10.10.1.3       4          520       9       5        1    0    0 00:02:40        1
```
Как видно из результатов проверки RR-server(R24) установил сессии с тремя остальными RR-client(R23, R25, R26), а RR-client(R25) только с RR-server(R25).  

### Часть 3. Настройте офис Москва так, чтобы приоритетным провайдером стал Ламас.  
Перед началом выполнения этого задания посмотрим как идет трафик с R14(AS 1001 Москва) к R18(AS 2042 СПб):  
`sh ip bgp`  
```
R14#sh ip bgp
BGP table version is 4, local router ID is 10.1.1.3
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>  10.1.1.3/32      0.0.0.0                  0         32768 i
 *>  10.1.1.5/32      10.1.2.34               11         32768 i
 *>  10.2.1.0/28      50.50.1.29                             0 101 301 520 2042
```
`traceroute` 
```
R14#traceroute 10.2.1.2 source 10.1.1.3 numeric
Type escape sequence to abort.
Tracing the route to 10.2.1.2
VRF info: (vrf in name/id, vrf out name/id)
  1 50.50.1.29 1 msec 10 msec 0 msec
  2 50.50.1.34 1 msec 0 msec 0 msec
  3 50.50.1.5 1 msec 0 msec 1 msec
  4 50.50.1.10 1 msec *  1 msec
```  
Как видно из вывода команд выбирается маршрут через AS 101 Киторн, необходимо сделать чтобы трафик шел через AS 301 Ламас.  

Для достижении данного условия создадим route-map на R14 который делает атрибут local-preference больше чем значение default(100):
```
R14#sh route-map SETLOCAL150
route-map SETLOCAL150, permit, sequence 10
  Match clauses:
  Set clauses:
    local-preference 150
  Policy routing matches: 0 packets, 0 bytes
```
После необходимо применить этот route-map к R15 для того чтобы маршруты получаемые через этот маршрутизатор, а соответственно маршруты через AS 301 Ламас получали больший приоритет чем маршруты приходящие от AS 101 Киторн:
```
R14#sh run | s bgp
router bgp 1001
 neighbor 10.1.1.5 route-map SETLOCAL150 in
```
Проведем проверку:
```
R14#sh ip bgp
BGP table version is 34, local router ID is 10.1.1.3
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>  10.1.1.3/32      0.0.0.0                  0         32768 i
 *>  10.1.1.5/32      10.1.2.34               11         32768 i
 *>  10.1.1.16/29     10.1.2.10               20         32768 ?
 *>  10.1.2.0/30      0.0.0.0                  0         32768 ?
 *>  10.1.2.8/30      0.0.0.0                  0         32768 ?
 *>  10.1.2.12/30     0.0.0.0                  0         32768 ?
 *>  10.1.2.16/30     10.1.2.10               20         32768 ?
 *>  10.1.2.24/30     10.1.2.14               20         32768 ?
 *>  10.1.2.28/30     10.1.2.34               20         32768 ?
 *>  10.1.2.32/30     0.0.0.0                  0         32768 ?
 *>  10.1.2.36/30     10.1.2.10               20         32768 ?
 *>  10.1.3.0/28      10.1.2.10               20         32768 ?
 *>  10.1.3.16/28     10.1.2.14               20         32768 ?
 * i 10.2.1.0/28      50.50.1.37               0    150      0 301 520 2042 i
     Network          Next Hop            Metric LocPrf Weight Path
 *>                   50.50.1.29                             0 101 301 520 2042 i
 * i 10.2.1.16/29     50.50.1.37               0    150      0 301 520 2042 i
 *>                   50.50.1.29                             0 101 301 520 2042 i
 * i 10.2.3.0/28      50.50.1.37               0    150      0 301 520 2042 i
 *>                   50.50.1.29                             0 101 301 520 2042 i
 * i 10.2.3.16/28     50.50.1.37               0    150      0 301 520 2042 i
 *>                   50.50.1.29                             0 101 301 520 2042 i
 * i 10.10.1.2/32     50.50.1.37               0    150      0 301 520 ?
 *>                   50.50.1.29                             0 101 301 520 ?
 * i 10.10.1.3/32     50.50.1.37               0    150      0 301 520 i
 *>                   50.50.1.29                             0 101 520 i
 * i 10.10.1.4/32     50.50.1.37               0    150      0 301 520 ?
 *>                   50.50.1.29                             0 101 520 ?
 * i 10.10.1.5/32     50.50.1.37               0    150      0 301 520 ?
 *>                   50.50.1.29                             0 101 520 ?
 * i 10.10.2.0/30     50.50.1.37               0    150      0 301 520 ?
 *>                   50.50.1.29                             0 101 301 520 ?
 * i 10.10.2.4/30     50.50.1.37               0    150      0 301 520 ?
     Network          Next Hop            Metric LocPrf Weight Path
 *>                   50.50.1.29                             0 101 520 ?
```

Видно что среди двух вариантов маршрутов во внешние сети из AS1001 Москва лучшим стал маршрут через AS 301 Ламас.

### Часть 4. Настройте офис С.-Петербург так, чтобы трафик до любого офиса распределялся по двум линкам одновременно.
Перед выполнением данного задания посмотрим вывод команд `sh ip route bgp` и `sh ip bgp` на маршрутизаторе R18 (AS 2042 СПб):  
```
R18#sh ip route bgp
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is 50.50.1.9 to network 0.0.0.0

      10.0.0.0/8 is variably subnetted, 12 subnets, 4 masks
B        10.1.1.3/32 [20/0] via 50.50.1.9, 00:05:30
B        10.1.1.5/32 [20/0] via 50.50.1.9, 00:05:30
B        10.10.1.3/32 [20/0] via 50.50.1.9, 00:00:06
R18#sh ip bgp
BGP table version is 19, local router ID is 10.2.1.2
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>  10.1.1.3/32      50.50.1.9                              0 520 301 1001 i
 *>  10.1.1.5/32      50.50.1.9                              0 520 301 101 1001 i
 *>  10.2.1.0/28      0.0.0.0                  0         32768 i
 *   10.10.1.3/32     50.50.1.13                             0 520 i
 *>                   50.50.1.9                0             0 520 i
```
Как видно в таблице маршрутов есть только 1 маршрут до loopback интерфейса R24(AS 520 Триада) 10.10.1.3/32.  

Для выполнения условия задания необходимо настроить на данном маршрутизаторе балансировку.  
Включается балансировка следующим образом:  
```
router bgp 2042
 bgp bestpath as-path multipath-relax
 maximum-paths 2
```
Далее выполним такую же проверку как и  в начале:  

```
R18#sh ip route bgp
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is 50.50.1.9 to network 0.0.0.0

      10.0.0.0/8 is variably subnetted, 12 subnets, 4 masks
B        10.1.1.3/32 [20/0] via 50.50.1.9, 00:09:07
B        10.1.1.5/32 [20/0] via 50.50.1.9, 00:09:07
B        10.10.1.3/32 [20/0] via 50.50.1.13, 00:00:13
                      [20/0] via 50.50.1.9, 00:00:13
R18#sh ip bgp
BGP table version is 20, local router ID is 10.2.1.2
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>  10.1.1.3/32      50.50.1.9                              0 520 301 1001 i
 *>  10.1.1.5/32      50.50.1.9                              0 520 301 101 1001 i
 *>  10.2.1.0/28      0.0.0.0                  0         32768 i
 *m  10.10.1.3/32     50.50.1.13                             0 520 i
 *>                   50.50.1.9                0             0 520 i
```
По результатам видно что теперь у нас 2 маршрута да искомого адреса. 

### Часть 5. Все сети в лабораторной работе должны иметь IP связность.
Так как на стэнде в локациях настроены IGP, необходимо настроить их редистрибьюцию в BGP процесс: 
В AS 1001 Москва настроен OSPF:  
```
router bgp 1001
 redistribute ospf 1
```
В AS 520 Триада настроен ISIS:  
```
router bgp 520
 redistribute isis level-1-2
```
В AS 2042 СПб настроен named EIGRP(Поэтому сети добавляются вручную):  
```
router bgp 2042
 network 10.2.1.0 mask 255.255.255.240
 network 10.2.1.16 mask 255.255.255.248
 network 10.2.3.0 mask 255.255.255.240
 network 10.2.3.16 mask 255.255.255.240
```
Для проверки настройки посмотри таблицу маршрутов на маршрутизаторе R21 AS 301 Ламас:
```
R21#sh ip route BGP
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is not set

      10.0.0.0/8 is variably subnetted, 25 subnets, 4 masks
B        10.1.1.3/32 [20/0] via 50.50.1.38, 01:36:49
B        10.1.1.5/32 [20/0] via 50.50.1.33, 01:38:28
B        10.1.1.16/29 [20/0] via 50.50.1.38, 00:42:57
B        10.1.2.0/30 [20/0] via 50.50.1.38, 00:53:11
B        10.1.2.8/30 [20/0] via 50.50.1.38, 00:53:11
B        10.1.2.12/30 [20/0] via 50.50.1.38, 00:53:11
B        10.1.2.16/30 [20/0] via 50.50.1.38, 00:53:11
B        10.1.2.24/30 [20/0] via 50.50.1.38, 00:53:11
B        10.1.2.28/30 [20/0] via 50.50.1.33, 00:53:11
B        10.1.2.32/30 [20/0] via 50.50.1.38, 00:53:11
B        10.1.2.36/30 [20/0] via 50.50.1.38, 00:53:11
B        10.1.3.0/28 [20/0] via 50.50.1.38, 00:42:22
B        10.1.3.16/28 [20/0] via 50.50.1.38, 00:39:34
B        10.2.1.0/28 [20/0] via 50.50.1.5, 05:22:43
B        10.2.1.16/29 [20/0] via 50.50.1.5, 00:05:52
B        10.2.3.0/28 [20/0] via 50.50.1.5, 00:05:20
B        10.2.3.16/28 [20/0] via 50.50.1.5, 00:04:50
B        10.10.1.2/32 [20/20] via 50.50.1.5, 00:37:22
B        10.10.1.3/32 [20/0] via 50.50.1.5, 01:03:44
B        10.10.1.4/32 [20/30] via 50.50.1.5, 00:37:22
B        10.10.1.5/32 [20/20] via 50.50.1.5, 00:37:22
B        10.10.2.0/30 [20/20] via 50.50.1.5, 00:37:22
B        10.10.2.4/30 [20/20] via 50.50.1.5, 00:37:22
```
Видно что ему приходят префиксы из всех AS.
_______
  - [Конфигурации устройств](https://github.com/Alnor23/OTUS_NETWORK/tree/main/labs/lab10_ibgp/config)


