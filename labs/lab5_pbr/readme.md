# Маршрутизация на основе политик (PBR)  
______  
## Задание:  
1. Настроите политику маршрутизации для сетей офиса.  
2. Распределите трафик между двумя линками с провайдером.  
3. Настроите отслеживание линка через технологию IP SLA.(только для IPv4)  
4. Настройте для офиса Лабытнанги маршрут по-умолчанию.  
### Схема  
![scheme](https://github.com/Alnor23/OTUS_NETWORK/blob/main/labs/lab5_pbr/screenshots/scheme.png)  
______  
### Часть 1. Настроите политику маршрутизации для сетей офиса.   
  Для маршрутизатора офиса Чокурдах была выполнена следующая настройка
### Часть 2. Распределите трафик между двумя линками с провайдером. 
В данном пункте предпологается что трафик от VPC30 будет направлен к R26, а трафик от VPC31 к R25.  
Для реализации данной задачи необходимо произвести следующие настройки на маршрутизаторе R28:
1. Создаем ACL соответстввенно сетям VLAN VPC
```
R28(config)#do sh access-lists
Standard IP access list VLAN_31
    10 permit 10.3.3.0, wildcard bits 0.0.0.15
Standard IP access list VLAN_32
    10 permit 10.3.3.16, wildcard bits 0.0.0.15
```
2. Настраиваем PBR соответственно условию
```
R28#sh route-map
route-map PBR, permit, sequence 10
  Match clauses:
    ip address (access-lists): VLAN_31
  Set clauses:
    ip next-hop 50.50.1.17
  Policy routing matches: 0 packets, 0 bytes
route-map PBR, permit, sequence 20
  Match clauses:
    ip address (access-lists): VLAN_32
  Set clauses:
    ip next-hop 50.50.1.21
  Policy routing matches: 0 packets, 0 bytes
```
