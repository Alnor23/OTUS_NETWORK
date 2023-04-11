# Избыточность локальных сетей. STP
____
## Задание:  
Часть 1. Создание сети и настройка основных параметров устройства  
Часть 2. Выбор корневого моста  
Часть 3. Наблюдение за процессом выбора протоколом STP порта, исходя из стоимости портов  
Часть 4. Наблюдение за процессом выбора протоколом STP порта, исходя из приоритета портов  
___
### Часть 1
В соответствии с требованием задания была построена сеть:  
![scheme](https://github.com/Alnor23/OTUS_NETWORK/blob/main/labs/lab2_stp/Screnshots/lab2_topology.png)  
Были выполнена настройка основных параметров устройств (S1,S2,S3):
- Отключен поиск DNS `no ip domain-lookup`
- Назначено имя устройства `hostname R1`
- - Назначен требуемый пароль на EXEC, ENABLE, консоль и линии VTY  
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
- Настроен logging synchronous для консольного канала  
- Итерфейс управления был создан соответствующей командой `interface Vlan1` 
- Также устройствам были выданы IP - адреса в соответствии с заданием  
 #### Результаты проверки связи между устройствами
 Проверка связи от S1 к S2,S3  
 ![S1](https://github.com/Alnor23/OTUS_NETWORK/blob/main/labs/lab2_stp/Screnshots/S1.png)  
 _____
 Проверка связи от S2 к S3  
 ![S2](https://github.com/Alnor23/OTUS_NETWORK/blob/main/labs/lab2_stp/Screnshots/S2.png)  
 _____
 ### Часть 2
