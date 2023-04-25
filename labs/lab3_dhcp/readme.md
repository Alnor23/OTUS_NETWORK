
# Реализация DHCPv4, DHCPv6
__________
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
