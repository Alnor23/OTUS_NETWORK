
# Реализация DHCPv4, DHCPv6
__________
# DНСPv4
## Задание:
Часть 1: Построение сети и настройка основных параметров устройства  
Часть 2: Настройка и проверка двух серверов DHCPv4 на R1  
Часть 3: Настройка и проверка DHCP-relay на R2  

#### Таблица адресации
Device |  Interface | IP Address  | Subnet Mask | Default Gateway
-------|------------|-------------|-------------|----------------
R1  | e0/0  | 10.0.0.1  | 255.255.255.252 |   N/A
R1  | e0/1 | N/A | N/A | N/A
R1  | e0/1.100  |  192.168.1.1 | 255.255.255.192 | N/A
R1  | e0/1.200  |  192.168.1.65 | 255.255.255.224 | N/A
R1  | e0/1.1000 | N/A | N/A | N/A
R2  | e0/0  | 10.0.0.2  | 255.255.255.252 | N/A
R2  | e0/1  |  192.168.1.97 | 255.255.255.240 | N/A
S1  | VLAN 200  |   |   |  
S2  | VLAN 1  |   |   |  
PC-A  | NIC | DHCP  | DHCP  | DHCP
PC-B  | NIC | DHCP  | DHCP  | DHCP
#### Таблица VLAN  
VLAN  | Name  | Interface Assigned
------|-------|-------------------
1 | N/A | S2: e0/0
100 | Clients | S1: e0/0
200 | Management  | S1: VLAN 200 
999 | Parking_Lot | S1: e0/2-4, S2: e0/2-4, R1-R2 e0/2-4
1000  | Native |  N/A
_____
### Часть 1
#### Шаг 1  
Неообходимо разбить предоставленную сеть 192.168.1.0/24 на подсети согласно следующим условиям:
  1. Подсеть A, поддерживающая 58 хостов (клиентская VLAN на R1) `Net 192.168.1.0/26 (255.255.255.192), Gateway 192.168.1.1`
  2. Подсеть B, поддерживающая 28 хостов (управляющая VLAN на R1) `Net 192.168.1.64/27 (255.255.255.224), Gateway 192.168.1.65`
  3. Подсеть C, поддерживающая 12 хостов (клиентская VLAN в R2) `Net 192.168.1.96/28 (255.255.255.240), Gateway 192.168.1.97`  

#### Шаг 2  
Подключение всех устройства в топологию приведенную в задании:  
  ![scheme](https://github.com/Alnor23/OTUS_NETWORK/blob/main/labs/lab3_dhcp/screenshots/l3_top.png)

#### Шаг 3  
Настройка основных параметров маршрутизаторов:
  1. Назначено имя устройства `hostname R1`  
  2. Отключен поиск DNS `no ip domain-lookup`  
  3. Назначен требуемый пароль на EXEC, ENABLE, консоль и линии VTY  
  ```
  enable secret class  
  line console 0  
  password cisco  
  login  
  exit  
  line vty 0 4  
  password cisco  
  login
  ```
  4. Зашифрованны все пароли `service password-encryption`
  5. Установлен баннер `banner motd ^C Unauthorized access is strictly prohibited and prosecuted to the full extent of the law.^C` 

#### Шаг 4
Настройка маршрутизацию между VLAN на R1
 1. Активировать интерфейс от R1 к S1, настроить субинтерфейсы для каждой VLAN в соответствии с требованиями таблицы IP-адресации  
```
interface Ethernet0/1
 no ip address
!
interface Ethernet0/1.100
 description gate vlan 100
 encapsulation dot1Q 100
 ip address 192.168.1.1 255.255.255.192
!
interface Ethernet0/1.200
 description gate vlan 200
 encapsulation dot1Q 200
 ip address 192.168.1.65 255.255.255.224
!
interface Ethernet0/1.1000
 description Native
 encapsulation dot1Q 1000
!
```
#### Шаг 5  
  Настроить интерфейсы маршрутизатора R2 в соответствии с требованиями таблицы IP-адресации, а также статическую маршрутизацию между уcтройствами R1 и R2. Настройка маршрута по умолчанию на каждом маршрутизаторе, указывающем на IP-адрес e0/0 на другом маршрутизаторе  
```
interface Ethernet0/0
 description link to R2
 ip address 10.0.0.1 255.255.255.252
```
```
interface Ethernet0/0
 description link to R1
 ip address 10.0.0.2 255.255.255.252
!
interface Ethernet0/1
 ip address 192.168.1.97 255.255.255.240
!
```
```
ip routing
```
```
ip route 0.0.0.0 0.0.0.0 10.0.0.2
```
```
ip route 0.0.0.0 0.0.0.0 10.0.0.1
```
Результатом выполнения этого шага является отправка запроса на адрес R2 e0/1 из R1
```
R1#ping 192.168.1.97
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.97, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
```
#### Шаг 6 
На данном этапе необходимо настроить основные параметры коммутаторов
  1. Назначено имя устройства `hostname S1`  
  2. Отключен поиск DNS `no ip domain-lookup`  
  3. Назначен требуемый пароль на EXEC, ENABLE, консоль и линии VTY  
  ```
  enable secret class  
  line console 0  
  password cisco  
  login  
  exit  
  line vty 0 4  
  password cisco  
  login
  ```
  4. Зашифрованны все пароли `service password-encryption`
  5. Установлен баннер `banner motd ^C Unauthorized access is strictly prohibited and prosecuted to the full extent of the law.^C`  
#### Шаг 7  
Создание VLAN на S1  
  1. На S1 были созданы VLAN согласно таблице а также настроены интерфейсы управления на S1 и S2 c указанием шлюза
  ```
  S1#sh vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Et0/1
100  Clients                          active    Et0/0
200  Management                       active
999  Parking_Lot                      active    Et0/2, Et0/3
1000 Native                           active
1002 fddi-default                     act/unsup
1003 token-ring-default               act/unsup
1004 fddinet-default                  act/unsup
1005 trnet-default                    act/unsup
```
```
interface Vlan200
 ip address 192.168.1.66 255.255.255.224
!
ip default-gateway 192.168.1.65
```
```
interface Vlan1
 ip address 192.168.1.98 255.255.255.240
!
ip default-gateway 192.168.1.97
```
  2. Также на S1 неиспользуемые порты были помещены в Vlan Parking_Lot и деактивированы, на S2 неиспользуемые порты были деактивированы  
```
S1#sh run | sec interf
interface Ethernet0/0
 switchport access vlan 100
 switchport mode access
 duplex auto
interface Ethernet0/1
 duplex auto
interface Ethernet0/2
 switchport access vlan 999
 switchport mode access
 shutdown
 duplex auto
interface Ethernet0/3
 switchport access vlan 999
 switchport mode access
 shutdown
 duplex auto
```
```
S2#sh run | sec interf
interface Ethernet0/0
 duplex auto
interface Ethernet0/1
 duplex auto
interface Ethernet0/2
 shutdown
 duplex auto
interface Ethernet0/3
 shutdown
 duplex auto
 ```
 #### Шаг 8
 Настройка порта S1 e0/1 подключенного к R1
  Для корректной работы необходимо настроить этот порт как транковый установить разрешенные VLAN 100, 200, 1000, в качестве native vlan установить VLAN 1000
  ```
  S1#sh run int e0/1
Building configuration...

Current configuration : 138 bytes
!
interface Ethernet0/1
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
 duplex auto
end
```
______
### Часть 2
#### Шаг 1
Настройка сервера DHCPv4 на R1:  
В данном пункте необходимо настроить два сервера DHCPv4 для подсетей  192.168.1.0/26 и 192.168.1.96/28 со следующими условиями
  1. Исключены первые пять адресов 
  2. Для каждого пула используется уникальное имя
  3. Указана сеть которую поддерживает этот DHCP-сервер
  4. Доменное имя ccna-lab.com
  5. Настроен шлюз в соответствии с подсетью
  6. Настроить время аренды на 2 дня 12 часов 30 минут
```
R1#sh run | sec dhcp
ip dhcp excluded-address 192.168.1.1 192.168.1.5
ip dhcp excluded-address 192.168.1.97 192.168.1.101
ip dhcp pool R1_Client_LAN
 network 192.168.1.0 255.255.255.192
 default-router 192.168.1.1
 domain-name ccna-lab.com
 lease 2 12 30
ip dhcp pool R2_Client_LAN
 network 192.168.1.96 255.255.255.240
 default-router 192.168.1.97
 domain-name ccna-lab.com
 lease 2 12 30
```
#### Шаг 2
Проверка конфигурации сервера DHCPv4  
Командой `sh ip dhcp pool` можно просмотреть какие пулы адрессов раздаются на данном DHCPv4 сервере  
```
R1#sh ip dhcp pool

Pool R1_Client_LAN :
 Utilization mark (high/low)    : 100 / 0
 Subnet size (first/next)       : 0 / 0
 Total addresses                : 62
 Leased addresses               : 1
 Pending event                  : none
 1 subnet is currently in the pool :
 Current index        IP address range                    Leased addresses
 192.168.1.7          192.168.1.1      - 192.168.1.62      1

Pool R2_Client_LAN :
 Utilization mark (high/low)    : 100 / 0
 Subnet size (first/next)       : 0 / 0
 Total addresses                : 14
 Leased addresses               : 0
 Pending event                  : none
 1 subnet is currently in the pool :
 Current index        IP address range                    Leased addresses
 192.168.1.97         192.168.1.97     - 192.168.1.110     0
```
Командой  `sh ip dhcp binding` какие адреса уже выданы DHCPv4 сервером   
```R1#sh ip dhcp binding
Bindings from all pools not associated with VRF:
IP address          Client-ID/              Lease expiration        Type
                    Hardware address/
                    User name
192.168.1.6         0100.5079.6668.01       Apr 28 2023 07:22 AM    Automatic
```
Командой  `show ip dhcp server statistics` можно посмотреть статистику по DHCPv4 серверу (такую как общее кол-во пулов, выданных адресов, кол-во запросов DORA и др.)  
```
R1#show ip dhcp server statistics
Memory usage         33634
Address pools        2
Database agents      0
Automatic bindings   1
Manual bindings      0
Expired bindings     0
Malformed messages   0
Secure arp entries   0

Message              Received
BOOTREQUEST          0
DHCPDISCOVER         2
DHCPREQUEST          1
DHCPDECLINE          0
DHCPRELEASE          0
DHCPINFORM           0

Message              Sent
BOOTREPLY            0
DHCPOFFER            1
DHCPACK              1
DHCPNAK              0
```
#### Шаг 3
Получение адреса устройством PC-A  
Устройство PC-A является устройство VPCS, для получения им адреса посредством DHCP необходимо сделать следующие действия  
```
VPCS> ip dhcp
DDORA IP 192.168.1.6/26 GW 192.168.1.1
```
Как видно PC-A получил адрес из пула (в этом можно убедиться также в выводе команды `sh ip dhcp binding` непосредственно на DHCPv4 сервере в пункте выше)  
____
### Часть 3
#### Шаг 1
В данной части необходимо настроить функционал ретрансляции DHCP запросов  
Интерфейс где это будет реализовано будет интерфейс e0/1 от R2 к S2 (так как на него будут поступать запросы от клиентов в сети), а в качестве адреса ретрансляции будет выступать адрес маршрутизатора R1
```
interface Ethernet0/1
 ip address 192.168.1.97 255.255.255.240
 ip helper-address 10.0.0.1
```
#### Шаг 2
После настройки DHCP ретрансляции устройство PC-B получило адрес
```
VPCS> ip dhcp
DDORA IP 192.168.1.102/28 GW 192.168.1.97
```
Также в этом можно убедиться на основании вывода команды `sh ip dhcp binding` на DHCPv4 сервере
```
R1#sh ip dhcp binding
Bindings from all pools not associated with VRF:
IP address          Client-ID/              Lease expiration        Type
                    Hardware address/
                    User name
192.168.1.6         0100.5079.6668.01       Apr 28 2023 07:22 AM    Automatic
192.168.1.102       0100.5079.6668.02       Apr 28 2023 07:58 AM    Automatic
```
______
# DНСPv6  
## Задание:  
Часть 1: Построение сети и настройка основных параметров устройства
Часть 2: Проверка назначения адреса SLAAC из R1
Часть 3: Настройка и проверка сервера DHCPv6 без состояния на R1
Часть 4: Настройка и проверка сервера DHCPv6 с отслеживанием состояния на R1
Часть 5: Настройка и проверка ретрансляции DHCPv6 на R2  

#### Таблица адресации  
Device 	| Interface	| IPv6 Address
--------|-----------|-------------
R1	| e0/0	| 2001:db8:acad:2::1 /64
R1	| e0/0	| fe80::1
R1	| e0/1	| 2001:db8:acad:1::1/64
R1	| e0/1	| fe80::1
R2	| e0/0	| 2001:db8:acad:2::2/64
R2	| e0/0	| fe80::2
R2	| e0/1	| 2001:db8:acad:3::1 /64
R2	| e0/1	| fe80::1
PC-A	| NIC	| DHCP
PC-B	| NIC	| DHCP
____
### Часть 1
Данная часть была выполнена в модуле DHCPv4, в данном задании из ранее не выполненного необходимо
  1. Включить маршрутизацию IPv6 на R1 и R2 командой `ipv6 unicast-routing`
  2. Настроить интерфейсы маршрутизаторов с адресами IPv6 в соответствии с таблицой адресации
```
R1(config)#do sh run | sec int
mmi polling-interval 60
interface Ethernet0/0
 description link to R2
 ip address 10.0.0.1 255.255.255.252
 ipv6 address FE80::1 link-local
 ipv6 address 2001:DB8:ACAD:2::1/64
interface Ethernet0/1
 no ip address
 ipv6 address FE80::1 link-local
 ipv6 address 2001:DB8:ACAD:1::1/64
```
```
R2(config)#do sh run | sec int
mmi polling-interval 60
interface Ethernet0/0
 description link to R1
 ip address 10.0.0.2 255.255.255.252
 ipv6 address FE80::2 link-local
 ipv6 address 2001:DB8:ACAD:2::2/64
interface Ethernet0/1
 ip address 192.168.1.97 255.255.255.240
 ip helper-address 10.0.0.1
 ipv6 address FE80::1 link-local
 ipv6 address 2001:DB8:ACAD:3::1/64
```
3. Настроить маршруты по умолчанию IPv6 для маршрутизаторов  
`R1(config)#ipv6 route ::/0 2001:db8:acad:2::2`  
`R2(config)#ipv6 route ::/0 2001:db8:acad:2::1`
### Часть 2  
Проверка назначения адреса SLAAC из R1  
В данной части PC-A получил адрес используя метод SLAAC
```
VPCS> ip auto
GLOBAL SCOPE      : 2001:db8:acad:1:2050:79ff:fe66:6801/64
ROUTER LINK-LAYER : aa:bb:cc:00:50:10

VPCS> sh ipv6

NAME              : VPCS[1]
LINK-LOCAL SCOPE  : fe80::250:79ff:fe66:6801/64
GLOBAL SCOPE      : 2001:db8:acad:1:2050:79ff:fe66:6801/64
DNS               :
ROUTER LINK-LAYER : aa:bb:cc:00:50:10
MAC               : 00:50:79:66:68:01
LPORT             : 20000
RHOST:PORT        : 127.0.0.1:30000
MTU:              : 1500
```
### Часть 3 
Настройка и проверка сервера DHCPv6 на R1  
В данной части необходимо настроить на маршрутизаторе R1 DHCPv6 сервер без сохранения состояний
```
R1#sh run int e0/1
Building configuration...

Current configuration : 171 bytes
!
interface Ethernet0/1
 no ip address
 ipv6 address FE80::1 link-local
 ipv6 address 2001:DB8:ACAD:1::1/64
 ipv6 nd other-config-flag
 ipv6 dhcp server R1-STATELESS
end

R1#sh run | sec ipv6 dhcp
ipv6 dhcp pool R1-STATELESS
 dns-server 2001:DB8:ACAD::254
 domain-name STATELESS.com
 ipv6 dhcp server R1-STATELESS
 ```
### Часть 4  
Настройка и проверка сервера DHCPv6 на R1
В данной части необходимо настроить на маршрутизаторе R3 DHCPv6 сервер c сохранением состояний, а также ретрансляцию DHCPv6 
```
R1#sh run | sec ipv6 dhcp
ipv6 dhcp pool R1-STATELESS
 dns-server 2001:DB8:ACAD::254
 domain-name STATELESS.com
ipv6 dhcp pool R2-STATEFUL
 address prefix 2001:DB8:ACAD:3:AAA::/80
 dns-server 2001:DB8:ACAD::254
 domain-name STATEFUL.com
 ipv6 dhcp server R2-STATEFUL
 ipv6 dhcp server R1-STATELESS
R1#sh run int e0/0
Building configuration...

Current configuration : 189 bytes
!
interface Ethernet0/0
 description link to R2
 ip address 10.0.0.1 255.255.255.252
 ipv6 address FE80::1 link-local
 ipv6 address 2001:DB8:ACAD:2::1/64
 ipv6 dhcp server R2-STATEFUL
end
```
```
R2#sh run int e0/1
Building configuration...

Current configuration : 256 bytes
!
interface Ethernet0/1
 ip address 192.168.1.97 255.255.255.240
 ip helper-address 10.0.0.1
 ipv6 address FE80::1 link-local
 ipv6 address 2001:DB8:ACAD:3::1/64
 ipv6 nd managed-config-flag
 ipv6 dhcp relay destination 2001:DB8:ACAD:2::1 Ethernet0/0
end
```
____
# Конфигурации устройств
- [Конфигурация S1](https://github.com/Alnor23/OTUS_NETWORK/blob/main/labs/lab3_dhcp/config/S1)  
- [Конфигурация S2](https://github.com/Alnor23/OTUS_NETWORK/blob/main/labs/lab3_dhcp/config/S2)  
- [Конфигурация R1](https://github.com/Alnor23/OTUS_NETWORK/blob/main/labs/lab3_dhcp/config/R1) 
- [Конфигурация R2](https://github.com/Alnor23/OTUS_NETWORK/blob/main/labs/lab3_dhcp/config/R2)
