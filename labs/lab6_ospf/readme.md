# OSPF
____
## Задание:
1. Маршрутизаторы R14-R15 находятся в зоне 0 - backbone.
2. Маршрутизаторы R12-R13 находятся в зоне 10. Дополнительно к маршрутам должны получать маршрут по умолчанию.
3. Маршрутизатор R19 находится в зоне 101 и получает только маршрут по умолчанию.
4. Маршрутизатор R20 находится в зоне 102 и получает все маршруты, кроме маршрутов до сетей зоны 101.
5. Настройка для IPv6 повторяет логику IPv4.
### Схема
![scheme](https://github.com/Alnor23/OTUS_NETWORK/blob/main/labs/lab6_ospf/screenshots/scheme_ospf.png)
### Таблица адресации  
Management
Device | IPv4 Address  | Subnet Mask    | IPv6 GLA Address         | IPv6 LLA Address
-------|---------------|----------------|--------------------------|-----------------
R19    |    10.1.1.2   |255.255.255.240 |20FF:0DB8:ACAD:1001::19/64|FE80::19/8
R14    |    10.1.1.3   |255.255.255.240 |20FF:0DB8:ACAD:1001::14/64|FE80::14/8   
R12    |    10.1.1.4   |255.255.255.240 |20FF:0DB8:ACAD:1001::12/64|FE80::12/8     
R15    |    10.1.1.5   |255.255.255.240 |20FF:0DB8:ACAD:1001::15/64|FE80::15/8 
R13    |    10.1.1.6   |255.255.255.240 |20FF:0DB8:ACAD:1001::13/64|FE80::13/8 
R20    |    10.1.1.7   |255.255.255.240 |20FF:0DB8:ACAD:1001::20/64|FE80::20/8 

Link
Device | Port | Address type | Address                      | Network
------ |------|--------------|------------------------------|---------
R19    | e0/0 | IPv4         | 10.1.2.1/30                  | 10.1.2.0/30
R19    | e0/0 | IPv6         | 20FF:0DB8:ACAD:1000::19:/64  | 20FF:0DB8:ACAD:1000::/64
R14    | e0/3 | IPv4         | 10.1.2.2/30                  | 10.1.2.0/30 
R14    | e0/3 | IPv6         | 20FF:0DB8:ACAD:1000::14:3/64 | 20FF:0DB8:ACAD:1000::/64
||||
R14    | e0/0 | IPv4         | 10.1.2.9/30                  | 10.1.2.8/30 
R14    | e0/0 | IPv6         | 20FF:0DB8:ACAD:1011::14:/64  | 20FF:0DB8:ACAD:1011::/64
R12    | e0/2 | IPv4         | 10.1.2.10/30                 | 10.1.2.8/30 
R12    | e0/2 | IPv6         | 20FF:0DB8:ACAD:1011::12:2/64 | 20FF:0DB8:ACAD:1011::/64
||||
R14    | e0/1 | IPv4         | 10.1.2.13/30                 | 10.1.2.12/30 
R14    | e0/1 | IPv6         | 20FF:0DB8:ACAD:1012::14:1/64 | 20FF:0DB8:ACAD:1012::/64
R13    | e0/3 | IPv4         | 10.1.2.14/30                 | 10.1.2.12/30 
R13    | e0/3 | IPv6         | 20FF:0DB8:ACAD:1012::13:3/64 | 20FF:0DB8:ACAD:1012::/64
||||
R12    | e0/3 | IPv4         | 10.1.2.17/30                 | 10.1.2.16/30 
R12    | e0/3 | IPv6         | 20FF:0DB8:ACAD:1013::12:3/64 | 20FF:0DB8:ACAD:1013::/64
R15    | e0/1 | IPv4         | 10.1.2.18/30                 | 10.1.2.16/30 
R15    | e0/1 | IPv6         | 20FF:0DB8:ACAD:1013::15:1/64 | 20FF:0DB8:ACAD:1013::/64
||||
R15    | e0/0 | IPv4         | 10.1.2.26/30                 | 10.1.2.24/30 
R15    | e0/0 | IPv6         | 20FF:0DB8:ACAD:1014::15:/64  | 20FF:0DB8:ACAD:1014::/64
R13    | e0/2 | IPv4         | 10.1.2.25/30                 | 10.1.2.24/30 
R13    | e0/2 | IPv6         | 20FF:0DB8:ACAD:1014::13:2/64 | 20FF:0DB8:ACAD:1014::/64
||||
R15    | e0/3 | IPv4         | 10.1.2.29/30                 | 10.1.2.28/30 
R15    | e0/3 | IPv6         | 20FF:0DB8:ACAD:1015::15:3/64 | 20FF:0DB8:ACAD:1015::/64
R20    | e0/0 | IPv4         | 10.1.2.30/30                 | 10.1.2.28/30 
R20    | e0/0 | IPv6         | 20FF:0DB8:ACAD:1015::20:/64  | 20FF:0DB8:ACAD:1015::/64
||||
R14    | e1/0 | IPv4         | 10.1.2.33/30                 | 10.1.2.32/30 
R14    | e1/0 | IPv6         | 20FF:0DB8:ACAD:1016::14:10/64| 20FF:0DB8:ACAD:1016::/64
R15    | e1/0 | IPv4         | 10.1.2.34/30                 | 10.1.2.32/30 
R15    | e1/0 | IPv6         | 20FF:0DB8:ACAD:1016::15:10/64| 20FF:0DB8:ACAD:1016::/64
||||
R12    | e0/1 | IPv4         | 10.1.2.37/30                 | 10.1.2.36/30 
R12    | e0/1 | IPv6         | 20FF:0DB8:ACAD:1017::12:1/64| 20FF:0DB8:ACAD:1017::/64
R13    | e0/1 | IPv4         | 10.1.2.38/30                 | 10.1.2.36/30 
R13    | e0/1 | IPv6         | 20FF:0DB8:ACAD:1017::13:1/64| 20FF:0DB8:ACAD:1017::/64
_________
### Часть 1. Маршрутизаторы R14-R15 находятся в зоне 0 - backbone.  
Для выполнения задания необходимо применить следующие настройки на маршрутизаторах R14-R15:  
  1. Включаем OSPF на устройстве и присваиваем ему router-id:  
     R14
     ```
     router ospf 1
     router-id 1.1.1.1
     ```
     R15
     ```
     router ospf 1
     router-id 2.2.2.2
     ```
  2. Включение OSPF непосредственно на интерфейсах между устройствами `ip ospf 1 area 0`.
     
### Часть 2. Маршрутизаторы R12-R13 находятся в зоне 10. Дополнительно к маршрутам должны получать маршрут по умолчанию.
В этой чассти в дополнение к стандартным настройкам из предыдущей части. Необходимо также настроить зону 10 вида stub для выполнения условий задания.  
  1. Создаем зону 10 типа stub командой `area 10 stub` на устройствах R14, R15, R12, R13.  
  2. Включаем на соответствующих интерфейсах маршрутизаторов `ip ospf 1 area 0`.
     Для проверки используем команду `show ip route ospf`  на маршрутизаторе R12:
```
R12#sh ip route ospf
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is 10.1.2.18 to network 0.0.0.0

O*IA  0.0.0.0/0 [110/11] via 10.1.2.18, 00:14:16, Ethernet0/3
                [110/11] via 10.1.2.9, 00:20:07, Ethernet0/2
      10.0.0.0/8 is variably subnetted, 17 subnets, 4 masks
O IA     10.1.2.0/30 [110/20] via 10.1.2.9, 00:20:07, Ethernet0/2
O        10.1.2.12/30 [110/20] via 10.1.2.38, 00:07:20, Ethernet0/1
                      [110/20] via 10.1.2.9, 00:07:30, Ethernet0/2
O        10.1.2.24/30 [110/20] via 10.1.2.38, 00:07:20, Ethernet0/1
                      [110/20] via 10.1.2.18, 00:07:30, Ethernet0/3
O IA     10.1.2.28/30 [110/20] via 10.1.2.18, 00:14:19, Ethernet0/3
O IA     10.1.2.32/30 [110/20] via 10.1.2.18, 00:14:16, Ethernet0/3

R12#sh ipv6 route ospf
IPv6 Routing Table - default - 15 entries
Codes: C - Connected, L - Local, S - Static, U - Per-user Static route
       B - BGP, HA - Home Agent, MR - Mobile Router, R - RIP
       H - NHRP, I1 - ISIS L1, I2 - ISIS L2, IA - ISIS interarea
       IS - ISIS summary, D - EIGRP, EX - EIGRP external, NM - NEMO
       ND - ND Default, NDp - ND Prefix, DCE - Destination, NDr - Redirect
       O - OSPF Intra, OI - OSPF Inter, OE1 - OSPF ext 1, OE2 - OSPF ext 2
       ON1 - OSPF NSSA ext 1, ON2 - OSPF NSSA ext 2, la - LISP alt
       lr - LISP site-registrations, ld - LISP dyn-eid, a - Application
OI  ::/0 [110/11]
     via FE80::14, Ethernet0/2
     via FE80::15, Ethernet0/3
OI  20FF:DB8:ACAD:1000::/64 [110/20]
     via FE80::14, Ethernet0/2
O   20FF:DB8:ACAD:1012::/64 [110/20]
     via FE80::13, Ethernet0/1
     via FE80::14, Ethernet0/2
O   20FF:DB8:ACAD:1014::/64 [110/20]
     via FE80::15, Ethernet0/3
     via FE80::13, Ethernet0/1
OI  20FF:DB8:ACAD:1015::/64 [110/20]
     via FE80::15, Ethernet0/3
OI  20FF:DB8:ACAD:1016::/64 [110/20]
     via FE80::14, Ethernet0/2
     via FE80::15, Ethernet0/3   
```
Из вывода команды мы видим что маршрутизатор получает маршруты и маршрут по умолчанию посредством OSPF (аналогично на R13).  
### Часть 3. Маршрутизатор R19 находится в зоне 101 и получает только маршрут по умолчанию.  
Также производим стандартные настройки устройства R19, и определяем 101 зону типа totally stub на маршрутизаторах R19,R14:
  1. Создаем зону 101 типа totally stub на маршрутизаторах R19,R14 командой `area 101 stub no-summary`.
  2. Включаем на соответствующих интерфейсах маршрутизаторов `ip ospf 1 area 101`.
    Для проверки используем команду `show ip route ospf` на маршрутизаторе R19: 
```
R19#sh ip route ospf
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is 10.1.2.2 to network 0.0.0.0

O*IA  0.0.0.0/0 [110/11] via 10.1.2.2, 00:14:32, Ethernet0/0
R19#sh ipv6 route ospf
IPv6 Routing Table - default - 6 entries
Codes: C - Connected, L - Local, S - Static, U - Per-user Static route
       B - BGP, HA - Home Agent, MR - Mobile Router, R - RIP
       H - NHRP, I1 - ISIS L1, I2 - ISIS L2, IA - ISIS interarea
       IS - ISIS summary, D - EIGRP, EX - EIGRP external, NM - NEMO
       ND - ND Default, NDp - ND Prefix, DCE - Destination, NDr - Redirect
       O - OSPF Intra, OI - OSPF Inter, OE1 - OSPF ext 1, OE2 - OSPF ext 2
       ON1 - OSPF NSSA ext 1, ON2 - OSPF NSSA ext 2, la - LISP alt
       lr - LISP site-registrations, ld - LISP dyn-eid, a - Application
OI  ::/0 [110/11]
     via FE80::14, Ethernet0/0
```
Из вывода команды мы видим что маршрутизатор получает только маршрут по умолчанию посредством OSPF.  

### Часть 4. Маршрутизатор R20 находится в зоне 102 и получает все маршруты, кроме маршрутов до сетей зоны 101.
В этом задание необходимо настроить фильтрацию маршрутов для зоны 102 (ее тип будет normal area) в соответствии с заданием:
  1. Включаем на соответствующих интерфейсах маршрутизаторов R15 и R20 `ip ospf 1 area 102`.
  2. Создаем prefix-list`ы для IPv4 и IPv6 согласно условию.
```
R20#sh run | i pre
ip prefix-list area101 seq 5 deny 10.1.2.0/30
ip prefix-list area101 seq 10 permit 0.0.0.0/0
ipv6 prefix-list area101v6 seq 5 deny 20FF:DB8:ACAD:1000::/64
ipv6 prefix-list area101v6 seq 10 permit ::/0
```
  3. Применяем созданные prefix-list к зоне 102:
IPv4  
```
area 102 filter-list prefix area101 in
```  
IPv6  
```
address-family ipv6 unicast
  area 102 filter-list prefix area101v6 in
 exit-address-family
```
### Часть 5. Настройка для IPv6 повторяет логику IPv4.
Настройка для IPv6 была произведена на всех устройствах данной лабораторной работы в логике работы IPv4  
Пример R15
```
router ospfv3 1
 router-id 2.2.2.2
 area 10 stub
```
```
ipv6 ospf 1 area 0
```
### P.S.
На всех маршрутизаторах стенда была выполненна настройка passive-interface(IPv4, IPv6) для исключения рассылки hello пакетов через неиспользуемые в маршрутизации интерфесы.  
Пример R19  
```
R19#sh run | sec ospf
 ip ospf 1 area 101
 ipv6 ospf 1 area 101
router ospfv3 1
 router-id 5.5.5.5
 area 101 stub no-summary
 !
 address-family ipv6 unicast
  passive-interface default
  no passive-interface Ethernet0/0
 exit-address-family
router ospf 1
 router-id 5.5.5.5
 area 101 stub no-summary
 passive-interface default
 no passive-interface Ethernet0/0
```
Также был изменен тип сети loopback интерфейса с "loopback" на "point to point" для распространения их как в фактических настройках.  
Пример R19  
```
R19#sh run int lo1
Building configuration...

Current configuration : 237 bytes
!
interface Loopback1
 description manage
 ip address 10.1.1.2 255.255.255.240
 ip ospf network point-to-point
 ipv6 address FE80::19 link-local
 ipv6 address 20FF:DB8:ACAD:1001::19/64
 ipv6 enable
 ipv6 ospf network point-to-point
end
```
_______
  - [Конфигурации устройств](https://github.com/Alnor23/OTUS_NETWORK/tree/main/labs/lab6_ospf/config)

