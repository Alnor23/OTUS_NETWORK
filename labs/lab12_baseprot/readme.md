# Основные протоколы сети интернет
______  
1. Настройте NAT(PAT) на R14 и R15. Трансляция должна осуществляться в адрес автономной системы AS1001.  
2. Настройте NAT(PAT) на R18. Трансляция должна осуществляться в пул из 5 адресов автономной системы AS2042.  
3. Настройте статический NAT для R20.  
4. Настройте NAT так, чтобы R19 был доступен с любого узла для удаленного управления.  
5*. Настройте статический NAT(PAT) для офиса Чокурдах.  
5. Настройте для IPv4 DHCP сервер в офисе Москва на маршрутизаторах R12 и R13. VPC1 и VPC7 должны получать сетевые настройки по DHCP.  
6. Настройте NTP сервер на R12 и R13. Все устройства в офисе Москва должны синхронизировать время с R12 и R13.  
   
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

### Часть 1. Настроите NAT(PAT) на R14 и R15. Трансляция должна осуществляться в адрес автономной системы AS1001.  
Для выполнения задания в начале необходимо настроить интерфейсы соответствующим образом:  
```
R14(config)#int e0/2
R14(config-if)#ip nat outside
R14(config)#int range  e0/0,e0/1,e0/3,e1/0
R14(config-if-range)#ip nat inside
```
Далее создается access-list в который входит множество сетей AS 1001(Москва)(10.1.0.0/16):  
```
R14#sh run | s acc
access-list 1 permit 10.1.0.0 0.0.255.255
```
Затем мы настраиваем трансляцию:  
```
R15(config)#do sh run | sec ip nat
ip nat inside source list 1 interface Ethernet0/2 overload
```
Для проверки выполним icmp запрос из AS 1001 на внешние адреса а затем выполним просмотр трансляций на R15:  
```
R15#sh ip nat translations
Pro Inside global      Inside local       Outside local      Outside global
icmp 50.50.1.38:16046  10.1.3.2:16046     50.50.1.37:16046   50.50.1.37:16046
icmp 50.50.1.38:16302  10.1.3.2:16302     50.50.1.37:16302   50.50.1.37:16302
icmp 50.50.1.38:16558  10.1.3.2:16558     50.50.1.37:16558   50.50.1.37:16558
icmp 50.50.1.38:16814  10.1.3.2:16814     50.50.1.37:16814   50.50.1.37:16814
icmp 50.50.1.38:17070  10.1.3.2:17070     50.50.1.37:17070   50.50.1.37:17070
```
(Настройки на R14 и R15 аналогичны).  

### Часть 2. Настройте NAT(PAT) на R18. Трансляция должна осуществляться в пул из 5 адресов автономной системы AS2042.    
Настроим интерфейсы на R18:   
```
R18(config)#int e0/2
R18(config-if)#ip nat outside
R18(config)#int range e0/0-1
R18(config-if-range)#ip nat inside
```
Далее создается access-list в который входит множество сетей AS2042 (СПб):  
```
R18(config)#do sh run | s acc
access-list 1 permit 10.2.0.0 0.0.255.255
```
И создается пул куда будут транслироваться эти адреса:
```
R18#sh run | s nat
ip nat pool pool1 50.50.1.81 50.50.1.85 netmask 255.255.255.248
```
Затем мы настраиваем трансляцию:  
```
R18#sh run | s nat
 ip nat inside source list 1 pool pool1 overload
```
Для проверки выполним icmp запрос из AS 2042 на внешние адреса а затем выполним просмотр трансляций на R18:  
```
R18#sh ip nat translations
Pro Inside global      Inside local       Outside local      Outside global
icmp 50.50.1.81:2      10.2.1.2:2         50.50.1.5:2        50.50.1.5:2
```
### Часть 3. Настройте статический NAT для R20.  
NAT будет настроен на трансляцию адреса loopback(10.1.1.7) во внешний адрес loopback интерфейса (50.50.1.99) R15.  
Настроим трансляцию на R15:  
```
R15#sh run | s nat
ip nat inside source static 10.1.1.7 50.50.1.99
```
Затем выполним просмотр трансляций:  
```
R15#sh ip nat translations
Pro Inside global      Inside local       Outside local      Outside global
--- 50.50.1.99         10.1.1.7           ---                ---

```
### Часть  4. Настройте NAT так, чтобы R19 был доступен с любого узла для удаленного управления.  
Для выполнения этого задания создадим на R15 loopback интерфейс:  
```
interface Loopback2
 ip address 50.50.1.100 255.255.255.255
 ip ospf network point-to-point
 ip ospf 1 area 0
end
```
Дальше создадим статический NAT (который транслирует адрес loopback интерфейса R19 во внешний адрес):  
```
R15#sh run | s nat
ip nat inside source static 10.1.1.2 50.50.1.100
```
Затем выполним попытку подключения по telnet к R19 c устройства R18 (находящемся в AS 2042 СПБ):  
```
R18#telnet 50.50.1.100 /source-interface lo1
Trying 50.50.1.100 ... Open


User Access Verification

Username: cisco
Password:
R19>
R19>exit

[Connection to 50.50.1.100 closed by foreign host]
```
Видно что подключение по telnet выполнено.  

### Часть 5*. Настройте статический NAT(PAT) для офиса Чокурдах.
Так как в офисе Чокурдах не настроены протоколы маршрутизации и из офиса имеется два выхода в этом задании будет реализована работа нат на основание PBR.  
Для выполнения задания в начале необходимо настроить интерфейсы соответствующим образом:  
```
R28(config)#int e0/1
R28(config-if)#ip nat outside
R28(config-subif)#ex
R28(config)#int e0/0
R28(config-if)#ip nat outside
R28(config-subif)#ex
R28(config)#int e0/2.30
R28(config-subif)#ip nat inside
R28(config-subif)#ex
R28(config)#int e0/2.31
R28(config-subif)#ip nat inside
R28(config-subif)#int e0/2.32
R28(config-subif)#ip nat inside
R28(config-subif)#end
```
Далее создается access-list в который входят все адреса Чокурдах (10.3.0.0/16) также создадим ACL описывающие сети VPC30 и VPC31:  
```
R28(config)#do sh run | s access
access-list 1 permit 10.3.0.0 0.0.255.255
access-list 2 permit 10.3.3.0 0.0.0.15
access-list 3 permit 10.3.3.16 0.0.0.15
```
Затем настраиваем SLA на мониторинг двух исходящих линков:  
```
R28#sh run | sec ip sla
ip sla 1
 icmp-echo 50.50.1.17 source-ip 50.50.1.18
ip sla schedule 1 life forever start-time now
ip sla 2
 icmp-echo 50.50.1.21 source-ip 50.50.1.22
ip sla schedule 2 life forever start-time now
```
Затем создаем track обьекты:  
```
R28#sh run | sec track1
track 1 ip sla 1 reachability
 delay down 1 up 1
track 2 ip sla 2 reachability
 delay down 1 up 1
```
И прявязываем эти обьекты к маршрутам по умолчанию(чтобы мыршрут по умолчанию уходил из таблицы при срабатывании SLA):  
```
ip route 0.0.0.0 0.0.0.0 50.50.1.17 track 1
ip route 0.0.0.0 0.0.0.0 50.50.1.21 track 2
```
Создадим route-map для распределения трафика между линками на основании source сети, а также сразу создадим route-map для указаний NAT`у:  
```
R28#sh run | s route-map
route-map PBR permit 10
 match ip address VLAN_31
 set ip next-hop verify-availability 50.50.1.17 1 track 1
route-map PBR permit 20
 match ip address VLAN_32
 set ip next-hop verify-availability 50.50.1.21 1 track 2
route-map NAT2 permit 10
 match ip address 1
 match interface Ethernet0/1
route-map NAT1 permit 10
 match ip address 1
 match interface Ethernet0/0

```
Затем мы настраиваем трансляции:   
```
R28(config)#do sh run | s nat inside
ip nat inside source route-map NAT1 interface Ethernet0/0 overload
ip nat inside source route-map NAT2 interface Ethernet0/1 overload
```
Выполним проверку ping с VPC 31 к обоим линкам (при включенных интерфейсах) и посмотрим трансляции:  
```
R28#sh ip nat translations
Pro Inside global      Inside local       Outside local      Outside global
icmp 50.50.1.22:877    10.3.3.18:877      50.50.1.17:877     50.50.1.17:877
icmp 50.50.1.22:1133   10.3.3.18:1133     50.50.1.17:1133    50.50.1.17:1133
icmp 50.50.1.22:1389   10.3.3.18:1389     50.50.1.17:1389    50.50.1.17:1389
icmp 50.50.1.22:1645   10.3.3.18:1645     50.50.1.17:1645    50.50.1.17:1645
icmp 50.50.1.22:1901   10.3.3.18:1901     50.50.1.17:1901    50.50.1.17:1901
icmp 50.50.1.22:3181   10.3.3.18:3181     50.50.1.21:3181    50.50.1.21:3181
icmp 50.50.1.22:3437   10.3.3.18:3437     50.50.1.21:3437    50.50.1.21:3437
icmp 50.50.1.22:3693   10.3.3.18:3693     50.50.1.21:3693    50.50.1.21:3693
icmp 50.50.1.22:3949   10.3.3.18:3949     50.50.1.21:3949    50.50.1.21:3949
icmp 50.50.1.22:4205   10.3.3.18:4205     50.50.1.21:4205    50.50.1.21:4205
```
Здесь видно что трансляция идет всегда через 50.50.1.22 согласно PBR.  
Выполним проверку ping с VPC 31 к обоим линкам (предварительно отключим тот линк(R28 e0/1) через который lдолжен выходить данный VPC по PBR) и посмотрим трансляции:  
```
R28#sh ip nat translations
Pro Inside global      Inside local       Outside local      Outside global
icmp 50.50.1.18:3950   10.3.3.18:3950     50.50.1.17:3950    50.50.1.17:3950
icmp 50.50.1.18:4206   10.3.3.18:4206     50.50.1.17:4206    50.50.1.17:4206
icmp 50.50.1.18:4462   10.3.3.18:4462     50.50.1.17:4462    50.50.1.17:4462
icmp 50.50.1.18:4718   10.3.3.18:4718     50.50.1.17:4718    50.50.1.17:4718
icmp 50.50.1.18:4974   10.3.3.18:4974     50.50.1.17:4974    50.50.1.17:4974
icmp 50.50.1.18:5998   10.3.3.18:5998     50.50.1.21:5998    50.50.1.21:5998
icmp 50.50.1.18:6254   10.3.3.18:6254     50.50.1.21:6254    50.50.1.21:6254
icmp 50.50.1.18:6510   10.3.3.18:6510     50.50.1.21:6510    50.50.1.21:6510
icmp 50.50.1.18:6766   10.3.3.18:6766     50.50.1.21:6766    50.50.1.21:6766
icmp 50.50.1.18:7022   10.3.3.18:7022     50.50.1.21:7022    50.50.1.21:7022
```
Как видно что адрес выхода изменился в связи с тем что тот который был назначен PBR стал недоступен, из этого следует что в данном упражнении трансляция будет работать не смотря на доступность интерфейсов, но при наличии обоих маршрутизация осуществляется средствами PBR.  
### Часть 5. Настройте для IPv4 DHCP сервер в офисе Москва на маршрутизаторах R12 и R13. VPC1 и VPC7 должны получать сетевые настройки по DHCP.  
В данном задании на каждом маршрутизаторе будут подняты оба DHCP пула для клиентов, также будет настроен VRRP:      
Настройка VRRP R12:  
```
R12#sh run int e0/0.11
Building configuration...

Current configuration : 198 bytes
!
interface Ethernet0/0.11
 encapsulation dot1Q 11
 ip address 10.1.3.1 255.255.255.240
 vrrp 11 ip 10.1.3.3
 vrrp 11 priority 120
 vrrp 11 track 1 decrement 100
 vrrp 11 track 2 decrement 100
end

R12#sh run int e0/0.12
Building configuration...

Current configuration : 162 bytes
!
interface Ethernet0/0.12
 encapsulation dot1Q 12
 vrrp 12 ip 10.1.3.19
 vrrp 12 priority 150
 vrrp 12 track 1 decrement 100
 vrrp 12 track 2 decrement 100
end

R12#sh run | s track
track 1 interface Ethernet0/2 line-protocol
track 2 interface Ethernet0/3 line-protocol
```
Настройка VRRP R13:  
```
R13#sh run int e0/0.11
Building configuration...

Current configuration : 114 bytes
!
interface Ethernet0/0.11
 encapsulation dot1Q 11
 ip address 10.1.3.2 255.255.255.240
 vrrp 11 ip 10.1.3.3
end

R13#sh run int e0/0.12
Building configuration...

Current configuration : 116 bytes
!
interface Ethernet0/0.12
 encapsulation dot1Q 12
 ip address 10.1.3.17 255.255.255.240
 vrrp 12 ip 10.1.3.19
end
```
Настройка VRRP R12:  
```
R12#sh run int e0/0.11
Building configuration...

Current configuration : 198 bytes
!
interface Ethernet0/0.11
 encapsulation dot1Q 11
 ip address 10.1.3.1 255.255.255.240
 vrrp 11 ip 10.1.3.3
 vrrp 11 priority 120
 vrrp 11 track 1 decrement 100
 vrrp 11 track 2 decrement 100
end

R12#sh run int e0/0.12
Building configuration...

Current configuration : 162 bytes
!
interface Ethernet0/0.12
 encapsulation dot1Q 12
 vrrp 12 ip 10.1.3.19
 vrrp 12 priority 150
 vrrp 12 track 1 decrement 100
 vrrp 12 track 2 decrement 100
end


```
Далее настроим DНCP.  
Настройка R12:  
```
R12#sh run | s dhcp
ip dhcp excluded-address 10.1.3.1 10.1.3.3
ip dhcp excluded-address 10.1.3.9 10.1.3.14
ip dhcp excluded-address 10.1.3.17 10.1.3.18
ip dhcp excluded-address 10.1.3.17 10.1.3.19
ip dhcp excluded-address 10.1.3.20 10.1.3.25
ip dhcp pool vpc1_cli
 network 10.1.3.0 255.255.255.240
 default-router 10.1.3.3
ip dhcp pool vpc7_cli
 network 10.1.3.16 255.255.255.240
 default-router 10.1.3.19s
```
Настройка R13: 
```
R13#sh run | s dhcp
ip dhcp excluded-address 10.1.3.1 10.1.3.3
ip dhcp excluded-address 10.1.3.17 10.1.3.19
ip dhcp excluded-address 10.1.3.4 10.1.3.6
ip dhcp excluded-address 10.1.3.26 10.1.3.30
ip dhcp pool vpc1_cli
 network 10.1.3.0 255.255.255.240
 default-router 10.1.3.3
ip dhcp pool vpc7_cli
 network 10.1.3.16 255.255.255.240
 default-router 10.1.3.19
```
Проверка VPC1 (при отключении одного из маршрутизаторов):  
```
VPCS> ip dhcp
DORA IP 10.1.3.4/28 GW 10.1.3.3

VPCS> sh ip

NAME        : VPCS[1]
IP/MASK     : 10.1.3.4/28
GATEWAY     : 10.1.3.3
DNS         :
DHCP SERVER : 10.1.3.1
DHCP LEASE  : 86201, 86400/43200/75600
MAC         : 00:50:79:66:68:01
LPORT       : 20000
RHOST:PORT  : 127.0.0.1:30000
MTU         : 1500

VPCS> ip dhcp
DORA IP 10.1.3.7/28 GW 10.1.3.3

VPCS> sh ip

NAME        : VPCS[1]
IP/MASK     : 10.1.3.7/28
GATEWAY     : 10.1.3.3
DNS         :
DHCP SERVER : 10.1.3.2
DHCP LEASE  : 86395, 86400/43200/75600
MAC         : 00:50:79:66:68:01
LPORT       : 20000
RHOST:PORT  : 127.0.0.1:30000
MTU         : 1500
```
### Часть 6. Настройте NTP сервер на R12 и R13. Все устройства в офисе Москва должны синхронизировать время с R12 и R13.   
Настроим устройства R12 и R13 как NTP сервера:  
```
R12(config)#ntp master 10
```
(На R13 настройка аналогичная).  
Далее на клиентах укажем сервера (в качестве адресов сервера используются loopback интерфейсы):  
```
R14#sh run | sec ntp
ntp server 10.1.1.4
ntp server 10.1.1.6
```
Выполним проверку:  
```
R14#sh ntp associations

  address         ref clock       st   when   poll reach  delay  offset   disp
*~10.1.1.4        127.127.1.1     10    597   1024   377  0.000   0.000  2.003
+~10.1.1.6        127.127.1.1     10    741   1024   377  0.000   0.000  1.990
```
_______
  - [Конфигурации устройств](https://github.com/Alnor23/OTUS_NETWORK/tree/main/labs/lab12_baseprot/config)




