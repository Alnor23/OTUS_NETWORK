# IS-IS 
______  
## Задание:  
1. Настроите IS-IS в ISP Триада.
2. R23 и R25 находятся в зоне 2222.
3. R24 находится в зоне 24.
4. R26 находится в зоне 26.
5. Настройка осуществляется одновременно для IPv4 и IPv6.
### Схема  
![scheme](https://github.com/Alnor23/OTUS_NETWORK/blob/main/labs/lab7_isis/screenshots/scheme.png) 
### Таблица адресации  
Management
Device | IPv4 Address  | Subnet Mask    | IPv6 GLA Address         | IPv6 LLA Address
-------|---------------|----------------|--------------------------|------------------
R23    |10.10.1.2      |255.255.255.248 |20FF:0DB8:ACAD:4001::23/64|FE80::23/8
R24    |10.10.1.3      |255.255.255.248 |20FF:0DB8:ACAD:4001::24/64|FE80::24/8
R25    |10.10.1.4      |255.255.255.248 |20FF:0DB8:ACAD:4001::25/64|FE80::25/8
R26    |10.10.1.5      |255.255.255.248 |20FF:0DB8:ACAD:4001::26/64|FE80::26/8

Link inside AS
Device | Port | Address type | Address                      | Network
------ |------|--------------|------------------------------|---------
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
### Настройте IS-IS в ISP Триада.  
Для выполнение задания необходимо выполнить соедующее:  
1. Включить IS-IS на маршрутизаторе: `router isis`  
2. Настроить NET адрес для работы IS-IS:  
   R23 `net 49.2222.0000.0000.0023.00`  
   R24 `net 49.2222.0000.0000.0025.00`  
   R25 `net 49.0024.0000.0000.0024.00`  
   R26 `net 49.0026.0000.0000.0026.00`  
   В этом пункте мы также настраиваем зоны, заполняя соответствующее поле в NET адресе.  
3. Включить IS-IS на интерфейсе:  
   IPv4 `ip router isis`  
   IPv6 `ipv6 router isis`
### Проверка работоспособности IS-IS в ISP Триада.  
Все проверки для примера будут выполнены на маршрутизаторе R23:  
Просмотр таблицы маршрутизации:  
```
R23#sh ip route isis
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

      10.0.0.0/8 is variably subnetted, 8 subnets, 3 masks
i L1     10.10.2.4/30 [115/20] via 10.10.2.2, 00:56:38, Ethernet0/1
i L2     10.10.2.8/30 [115/20] via 10.10.2.13, 00:52:51, Ethernet0/2
R23#sh ipv6 route isis
IPv6 Routing Table - default - 11 entries
Codes: C - Connected, L - Local, S - Static, U - Per-user Static route
       B - BGP, HA - Home Agent, MR - Mobile Router, R - RIP
       H - NHRP, I1 - ISIS L1, I2 - ISIS L2, IA - ISIS interarea
       IS - ISIS summary, D - EIGRP, EX - EIGRP external, NM - NEMO
       ND - ND Default, NDp - ND Prefix, DCE - Destination, NDr - Redirect
       O - OSPF Intra, OI - OSPF Inter, OE1 - OSPF ext 1, OE2 - OSPF ext 2
       ON1 - OSPF NSSA ext 1, ON2 - OSPF NSSA ext 2, la - LISP alt
       lr - LISP site-registrations, ld - LISP dyn-eid, a - Application
I1  20FF:DB8:ACAD:4012::/64 [115/20]
     via FE80::25, Ethernet0/1
I2  20FF:DB8:ACAD:4013::/64 [115/20]
     via FE80::24, Ethernet0/2
```
Просмотр соседей:  
```
R23#sh isis neighbors

System Id      Type Interface   IP Address      State Holdtime Circuit Id
R24            L2   Et0/2       10.10.2.13      UP    8        R24.02           
R25            L1   Et0/1       10.10.2.2       UP    7        R25.02           
R25            L2   Et0/1       10.10.2.2       UP    9        R25.02
```
______
- [Конфигурации устройств](https://github.com/Alnor23/OTUS_NETWORK/tree/main/labs/lab7_isis/config)
