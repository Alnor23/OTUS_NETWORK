# BGP. Основы
________
## Задание: 
1. Настройте eBGP между офисом Москва и двумя провайдерами - Киторн и Ламас.
2. Настройте eBGP между провайдерами Киторн и Ламас.
3. Настройте eBGP между Ламас и Триада.
4. Настройте eBGP между офисом С.-Петербург и провайдером Триада.
5. Организуйте IP доступность между пограничным роутерами офисами Москва и С.-Петербург.

### Схема
![scheme](https://github.com/Alnor23/OTUS_NETWORK/blob/main/labs/lab9_bgp/screenshots/Scheme.png)

### Таблица адресации
Management
Device | IPv4 Address  | Subnet Mask    | IPv6 GLA Address         | IPv6 LLA Address | AS
-------|---------------|----------------|--------------------------|----------------- | -------
R14    |10.1.1.3       |255.255.255.240 |20FF:0DB8:ACAD:1001::14/64|FE80::14/8        |1001(Москва)
R15    |10.1.1.5       |255.255.255.240 |20FF:0DB8:ACAD:1001::15/64|FE80::15/8        |1001(Москва)
R18    |10.2.1.2       |255.255.255.240 |20FF:0DB8:ACAD:2001::18/64|FE80::18/8        |1001(СПб)
R23    |10.10.1.2      |255.255.255.248 |20FF:0DB8:ACAD:4001::23/64|FE80::23/8        |520(Триада)
R24    |10.10.1.3      |255.255.255.248 |20FF:0DB8:ACAD:4001::24/64|FE80::24/8        |520(Триада)
R25    |10.10.1.4      |255.255.255.248 |20FF:0DB8:ACAD:4001::25/64|FE80::25/8        |520(Триада)
R26    |10.10.1.5      |255.255.255.248 |20FF:0DB8:ACAD:4001::26/64|FE80::26/8        |520(Триада)
R22    |10.11.1.2      |255.255.255.248 |20FF:ODB8:ACAD:5001::22/64|FE80::22/8        |101(Киторн)
R21    |10.12.1.2      |255.255.255.248 |20FF:0DB8:ACAD:6001::21/64|FE80::21/8        |301(Ламас)

Link
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

### Часть 1. Настройте eBGP между офисом Москва и двумя провайдерами - Киторн и Ламас.  
  Для выполнения данного задания необходимо на маршрутизаторах R14, R15 установить соседство с маршрутизаторами R22 и R21 соответственно:  
R14   
```
R14#sh run | s bgp
router bgp 1001
 bgp router-id 10.10.1.3
 bgp log-neighbor-changes
 neighbor 50.50.1.29 remote-as 101
```
R15  
```
R15#sh run | s bgp
router bgp 1001
 bgp router-id 10.10.1.5
 bgp log-neighbor-changes
 neighbor 50.50.1.37 remote-as 301
```
R22  
```
R22#sh run | sec bgp
router bgp 101
 bgp router-id 10.11.1.2
 bgp log-neighbor-changes
 neighbor 50.50.1.30 remote-as 1001
```
R21  
```
R21#sh run | s bgp
router bgp 301
 bgp router-id 10.12.1.2
 bgp log-neighbor-changes
 neighbor 50.50.1.38 remote-as 1001
```
В качестве bgp router-id выступает management loopback маршрутизатора.  
Для проверки установления соседства воспользуемся командой `sh ip bgp summary` на маршрутизаторе R14:  
```
R14#sh ip bgp summary
BGP router identifier 10.10.1.3, local AS number 1001
BGP table version is 1, main routing table version 1

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
50.50.1.29      4          101      37      38        1    0    0 00:30:35        0
```
На остальных вывод команды аналогичен.  

### Часть 2. Настройте eBGP между провайдерами Киторн и Ламас.  
В этой части необходимо в BGP процесс каждого маршрутизатора добавить друг друга в качестве соседа:  
R22  
```
neighbor 50.50.1.34 remote-as 301
```
R21  
```
neighbor 50.50.1.33 remote-as 101
```
Выполним проверку на R22(на R21 вывод аналогичен): 
```
R22#sh ip bgp summary
BGP router identifier 10.11.1.2, local AS number 101
BGP table version is 1, main routing table version 1

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
50.50.1.30      4         1001      53      52        1    0    0 00:44:22        0
50.50.1.34      4          301       5       5        1    0    0 00:03:31        0
```
### Часть 3. Настройте eBGP между Ламас и Триада.  
В этой части выполним добавление соседа маршрутизатору R21 и настройку R24:  
R21   
```
neighbor 50.50.1.5 remote-as 520
```
R24  
```
R24#sh run | s bgp
router bgp 520
 bgp router-id 10.10.1.3
 bgp log-neighbor-changes
 neighbor 50.50.1.6 remote-as 301
```
Выполним проверку на R21:  
```
R21#sh ip bgp summary
BGP router identifier 10.12.1.2, local AS number 301
BGP table version is 1, main routing table version 1

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
50.50.1.5       4          520       5       3        1    0    0 00:01:13        0
50.50.1.33      4          101      30      30        1    0    0 00:25:43        0
50.50.1.38      4         1001      64      64        1    0    0 00:54:55        0
```
### Часть 4. Настройте eBGP между офисом С.-Петербург и провайдером Триада.  
В этой части выполним добавление соседа маршрутизатору R24 и настройку R18:  
R24  
```
neighbor 50.50.1.10 remote-as 2042
```
R18  
```
R18#sh run | s bgp
router bgp 2042
 bgp router-id 10.2.1.2
 bgp log-neighbor-changes
 neighbor 50.50.1.9 remote-as 520
```
Выполним проверку на R24:  
```
R24#sh ip bgp summary
BGP router identifier 10.10.1.3, local AS number 520
BGP table version is 1, main routing table version 1

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
50.50.1.6       4          301      13      15        1    0    0 00:10:52        0
50.50.1.10      4         2042       5       3        1    0    0 00:01:11        0
```
### Часть 5. Организуйте IP доступность между пограничным роутерами офисами Москва и С.-Петербург.  
Для выполнение этого задания необходимо добавить префиксы на маршрутизаторе R14(Москва) и R18(С.-Петербург):  
R14  
```
network 10.1.1.0 mask 255.255.255.240
```
R18  
```
network 10.2.1.0 mask 255.255.255.240
```
Для проверки выполним команду ping c loopback R18 на loopback R14:  
```
R18#ping 10.1.1.3 source 10.2.1.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.1.1.3, timeout is 2 seconds:
Packet sent with a source address of 10.2.1.2
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
R18#
```
Из вывода команды видно что настроена доступность между маршрутизаторами офисов.
 _______
  - [Конфигурации устройств](https://github.com/Alnor23/OTUS_NETWORK/tree/main/labs/lab9_bgp/config)


