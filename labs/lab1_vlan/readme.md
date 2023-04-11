# VLAN и маршрутизация между VLAN
_____
## Задание:
Часть 1: Построение сети и настройка основных параметров устройства  
Часть 2: Создание VLAN и назначение портов коммутатора  
Часть 3: Настройка магистрали 802.1Q между коммутаторами  
Часть 4: Настройка маршрутизации между VLAN на маршрутизаторе
___
### Часть 1
В соответствии с требованием задания была построена сеть:  
![scheme](https://github.com/Alnor23/OTUS_NETWORK/blob/main/labs/lab1_vlan/lab1_scheme.png)  
Были выполнена настройка основных параметров устройств (R1,S1,S2):  
- Назначено имя устройства `hostname R1`  
- Отключен поиск DNS `no ip domain-lookup`
- Назначен требуемый пароль на EXEC, ENABLE, консоль и линии VTY  
`enable secret class`  
`line console 0`  
`password cisco`  
`login`  
`exit`  
`line vty 0 4`  
`password cisco`  
`login`  
- Зашифрованны все пароли `service password-encryption`  
- Установлен баннер `banner motd ^C Unauthorized access is strictly prohibited and prosecuted to the full extent of the law.^C`  
### Часть 2
Были проведены все настройки коммутаторов согласно заданию
- Создание VLAN из списка и присвоение им соответствующих имен:
![vlan_brief](https://github.com/Alnor23/OTUS_NETWORK/blob/main/labs/lab1_vlan/vlan_brief.png)
- Создание и настройка интерфейса управления;  
Итерфейс управления был создан соответствующей командой  
 `interface Vlan3`
- Настроен интерфейс управления и шлюз по умолчанию;  
 ![vlan_brief](https://github.com/Alnor23/OTUS_NETWORK/blob/main/labs/lab1_vlan/mgm_vlan.png)  
- Все неиспользуемые порты были назначены в отдельный VLAN и отключены;  
 Порты были назначены VLAN 7 ParkingLot и административно отключены;
### Часть 3  
В этой части были настроены порты соединяющие коммутаторы S1 и S2, а также уоммутатор S1 с маршрутизатором R1: 
`switchport trunk encapsulation dot1q`  
`switchport trunk native vlan 8`  
`switchport trunk allowed vlan 3,4,8`  
`duplex auto`  
### Часть 4
Также была настроена маршрутизация между VLAN на R1:  
`interface Ethernet0/0`  
 `no ip address`  
`!`  
`interface Ethernet0/0.3`  
`description Gateway vlan 3`  
`encapsulation dot1Q 3`  
`ip address 192.168.3.1 255.255.255.0`  
`!`  
`interface Ethernet0/0.4`  
`description Gateway vlan 4`  
`encapsulation dot1Q 4`  
`ip address 192.168.4.1 255.255.255.0`  
`!`  
`interface Ethernet0/0.8`  
`description native vlan`  
`encapsulation dot1Q 8`  
`!`  
## Результаты
___
Результатом выполнения данной лабораторной работы являются выполнение тестов на проверку доступности с хостов PC-A и PC-B(согласно заданию лабораторной):  
![PC-A](https://github.com/Alnor23/OTUS_NETWORK/blob/main/labs/lab1_vlan/PC-A.png)

# Конфигурации устройств
- [Конфигурация R1](https://github.com/Alnor23/OTUS_NETWORK/blob/main/labs/lab1_vlan/config/R1)  
- [Конфигурация S1](https://github.com/Alnor23/OTUS_NETWORK/blob/main/labs/lab1_vlan/config/S1)  
- [Конфигурация S2](https://github.com/Alnor23/OTUS_NETWORK/blob/main/labs/lab1_vlan/config/S2)  






