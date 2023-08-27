# BGP. Фильтрация
______  
## Задание: 
1. Настроить фильтрацию в офисе Москва так, чтобы не появилось транзитного трафика(As-path).  
2. Настроить фильтрацию в офисе С.-Петербург так, чтобы не появилось транзитного трафика(Prefix-list).  
3. Настроить провайдера Киторн так, чтобы в офис Москва отдавался только маршрут по умолчанию.  
4. Настроить провайдера Ламас так, чтобы в офис Москва отдавался только маршрут по умолчанию и префикс офиса С.-Петербург.  
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

### Часть 1. Настроить фильтрацию в офисе Москва так, чтобы не появилось транзитного трафика(As-path).  

Для начала посмотрим какие префиксы AS 1001(Москва)R14 анонсирует соседу AS 101(Киторн)R22:
```
R14#sh ip bgp neighbors 50.50.1.29 advertised-routes
BGP table version is 60, local router ID is 10.1.1.3
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
 *>i 10.2.1.0/28      10.1.1.5                 0    150      0 301 520 2042 i
 *>i 10.2.1.16/29     10.1.1.5                 0    150      0 301 520 2042 i
     Network          Next Hop            Metric LocPrf Weight Path
 *>i 10.2.3.0/28      10.1.1.5                 0    150      0 301 520 2042 i
 *>i 10.2.3.16/28     10.1.1.5                 0    150      0 301 520 2042 i
 *>i 10.10.1.2/32     10.1.1.5                 0    150      0 301 520 ?
 *>i 10.10.1.3/32     10.1.1.5                 0    150      0 301 520 i
 *>i 10.10.1.4/32     10.1.1.5                 0    150      0 301 520 ?
 *>i 10.10.1.5/32     10.1.1.5                 0    150      0 301 520 ?
 *>i 10.10.2.0/30     10.1.1.5                 0    150      0 301 520 ?
 *>i 10.10.2.4/30     10.1.1.5                 0    150      0 301 520 ?

Total number of prefixes 23
```
Как видно мы анонсируем не только префиксы нашей AS (10.1.0.0/16), так что теоретически можем стать транзитной AS.
Во первых необходимо создать as-path access-list, условиями которого будут только маршруты с пустым значением as-path (так как значение с каким либо значением AS-path оригинировались не в исходной AS):
```
R14(config)#do sh run | sec as-path
ip as-path access-list 1 permit ^$
ip as-path access-list 1 deny .*
```
Применим созданный acl к вновь созданному route-map и назначим его соседу AS 101(Киторн)R22:
```
R14#sh run | section route-map
 neighbor 10.1.1.5 route-map SETLOCAL150 in
 neighbor 50.50.1.29 route-map NOTRANSAS out
route-map SETLOCAL150 permit 10
 set local-preference 150
route-map NOTRANSAS permit 10
 match as-path 1
```
После этого еще раз посмотрим какие префиксы мы анонсируем:  
```
R14#sh ip bgp neighbors 50.50.1.29 advertised-routes
BGP table version is 80, local router ID is 10.1.1.3
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
```
Как видно мы анонсируем только префиксы нашей AS (10.1.0.0/16).  
Для выполнения задания аналогичную настройку выполним и на R15:  
```
R15#sh run | section route-map
 neighbor 50.50.1.37 route-map NOTRANSAS out
route-map NOTRANSAS permit 10
 match as-path 1
```
### Часть 2. Настроить фильтрацию в офисе С.-Петербург так, чтобы не появилось транзитного трафика(Prefix-list).  
Посмотрим какие префиксы анонсирует AS 2042(СПб)R18:
```
R18#sh ip bgp neighbors 50.50.1.9 advertised-routes
BGP table version is 132, local router ID is 10.2.1.2
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>  10.1.1.3/32      50.50.1.13                             0 520 301 1001 i
 *>  10.1.1.5/32      50.50.1.13                             0 520 301 1001 i
 *>  10.1.1.16/29     50.50.1.13                             0 520 301 1001 ?
 *>  10.1.2.0/30      50.50.1.13                             0 520 301 1001 ?
 *>  10.1.2.8/30      50.50.1.13                             0 520 301 1001 ?
 *>  10.1.2.12/30     50.50.1.13                             0 520 301 1001 ?
 *>  10.1.2.16/30     50.50.1.13                             0 520 301 1001 ?
 *>  10.1.2.24/30     50.50.1.13                             0 520 301 1001 ?
 *>  10.1.2.28/30     50.50.1.13                             0 520 301 1001 ?
 *>  10.1.2.32/30     50.50.1.13                             0 520 301 1001 ?
 *>  10.1.2.36/30     50.50.1.13                             0 520 301 1001 ?
 *>  10.1.3.0/28      50.50.1.13                             0 520 301 1001 ?
 *>  10.1.3.16/28     50.50.1.13                             0 520 301 1001 ?
 *>  10.2.1.0/28      0.0.0.0                  0         32768 i
     Network          Next Hop            Metric LocPrf Weight Path
 *>  10.2.1.16/29     10.2.2.1           1536000         32768 i
 *>  10.2.3.0/28      10.2.2.1           1536000         32768 i
 *>  10.2.3.16/28     10.2.2.6           1536000         32768 i
 *>  10.10.1.2/32     50.50.1.13                             0 520 ?
 *>  10.10.1.3/32     50.50.1.9                0             0 520 i
 *>  10.10.1.4/32     50.50.1.13                             0 520 ?
 *>  10.10.1.5/32     50.50.1.13                             0 520 ?
 *>  10.10.2.0/30     50.50.1.13                             0 520 ?
 *>  10.10.2.4/30     50.50.1.13                             0 520 ?

Total number of prefixes 23
```
Как видно мы анонсируем не только префиксы нашей AS (10.2.0.0/16).  
По условию задания необходимо чтобы анонсировались только сети оригинированные в данной AS c помощью prefix-list.  
Создадим необходимый prefix-list и привяжем его к route-map, и в дальнейшем назначим этот route-map на соседей:  
```
R18#sh run | s prefix-list
ip prefix-list 1 seq 5 permit 10.2.0.0/16 ge 17
 match ip address prefix-list 1

R18#sh run | s route-map
 neighbor 50.50.1.9 route-map NOTRANSAS out
 neighbor 50.50.1.13 route-map NOTRANSAS out
route-map NOTRANSAS permit 10
 match ip address prefix-list 1
```
После этого еще раз посмотрим какие префиксы мы анонсируем:  
```
R18#sh ip bgp neighbors 50.50.1.9 advertised-routes
BGP table version is 132, local router ID is 10.2.1.2
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>  10.2.1.0/28      0.0.0.0                  0         32768 i
 *>  10.2.1.16/29     10.2.2.1           1536000         32768 i
 *>  10.2.3.0/28      10.2.2.1           1536000         32768 i
 *>  10.2.3.16/28     10.2.2.6           1536000         32768 i

Total number of prefixes 4
```
Как видно мы анонсируем только префиксы нашей AS (10.2.0.0/16). 

