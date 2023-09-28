# IPSec over DmVPN  
______  
1. Настройте GRE поверх IPSec между офисами Москва и С.-Петербург.  
2. Настройте DMVPN поверх IPSec между Москва и Чокурдах, Лабытнанги.  
Дополнительно: Для IPSec использовать CA и сертификаты.  
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
### Настройка CA и Peers
Для выполнения данной лабораторной работы неообходимо настроить аутентификацию клиентов IPsec через сертификаты.  
В роли СA будет выступать R24(выполним на нем необходимые настройки:  
```
R24(config)#ip domain name LAB.RU
R24(config)#ip http server
R24(config)#crypto key generate rsa general-keys label CA exportable modulus 2048
R24(config)#crypto pki server CA
R24(cs-server)#no shut
```
Далее настроим клиентов:  
```
R15(config)#crypto key generate rsa label VPN modulus 2048
R15(config)#crypto pki trustpoint VPN
R15(ca-trustpoint)#enrollment url http://10.10.1.3
R15(ca-trustpoint)#subject-name CN=R15, OU=VPN, O=LAB, C=RU
R15(ca-trustpoint)#rsakeypair VPN
R15(ca-trustpoint)#revocation-check none
```
Затем настроим запрос сертификата CA и сгенерируем CSR и отправим его на подпись:  
```
R15(config)#crypto pki authenticate VPN
Certificate has the following attributes:
       Fingerprint MD5: 96A68334 770CB563 ED4F9AEC BFCBFF58
      Fingerprint SHA1: B69E2104 B9593695 27AEE8FB 86D618C1 4B737913

% Do you accept this certificate? [yes/no]: yes
Trustpoint CA certificate accepted.
R15(config)#crypto pki enroll
R15(config)#crypto pki enroll VPN
%
% Start certificate enrollment ..
% Create a challenge password. You will need to verbally provide this
   password to the CA Administrator in order to revoke your certificate.
   For security reasons your password will not be saved in the configuration.
   Please make a note of it.

Password:
Re-enter password:

% The subject name in the certificate will include: CN=R15, OU=VPN, O=LAB, C=RU
% The subject name in the certificate will include: R15
% Include the router serial number in the subject name? [yes/no]: n
% Include an IP address in the subject name? [no]: y
Enter Interface name or IP Address[]: 50.50.1.38
Request certificate from CA? [yes/no]: y
% Certificate request sent to Certificate Authority
% The 'show crypto pki certificate verbose VPN' commandwill show the fingerprint
```
На CA мы подписываем CSR и выпускаем сертификат:  
```
R24#crypto pki server CA grant all
```
Посмотрим результат на клиенте:  
```
R15#sh crypto pki cer
Certificate
  Status: Available
  Certificate Serial Number (hex): 02
  Certificate Usage: General Purpose
  Issuer:
    cn=CA
  Subject:
    Name: R15
    IP Address: 50.50.1.38
    hostname=R15+ipaddress=50.50.1.38
    cn=R15
    ou=VPN
    o=LAB
    c=RU
  Validity Date:
    start date: 19:18:49 UTC Sep 27 2023
    end   date: 19:18:49 UTC Sep 26 2024
  Associated Trustpoints: VPN

CA Certificate
  Status: Available
  Certificate Serial Number (hex): 01
  Certificate Usage: Signature
  Issuer:
    cn=CA
  Subject:
    cn=CA
  Validity Date:
    start date: 18:15:37 UTC Sep 27 2023
    end   date: 18:15:37 UTC Sep 26 2026
  Associated Trustpoints: VPN
```

### Часть 1. Настройте GRE поверх IPSec между офисами Москва и С.-Петербург.    
GRE между офисами был настроен в предыдущей лабораторной работе:  
R15:  
```
R15#sh run int tunnel 1
Building configuration...

Current configuration : 171 bytes
!
interface Tunnel1
 ip address 10.1.5.1 255.255.255.252
 ip mtu 1400
 ip tcp adjust-mss 1360
 keepalive 3 3
 tunnel source 50.50.1.38
 tunnel destination 50.50.1.10
end
```
R18:  
```
R18#sh run int tunnel 1
Building configuration...

Current configuration : 171 bytes
!
interface Tunnel1
 ip address 10.1.5.2 255.255.255.252
 ip mtu 1400
 ip tcp adjust-mss 1360
 keepalive 3 3
 tunnel source 50.50.1.10
 tunnel destination 50.50.1.38
end
```
Перейдем к настройке IPsec и настроим первую фазу:  
```
R15#sh run | sec crypto ikev2
crypto ikev2 proposal PH1
 encryption aes-cbc-128
 integrity md5
 group 2
crypto ikev2 policy IKEV2
 proposal PH1
crypto ikev2 profile PROF1
 match address local 50.50.1.38
 match identity remote address 50.50.1.10 255.255.255.255
 authentication remote rsa-sig
 authentication local rsa-sig
 pki trustpoint VPN
```
Далее создадим ACL для отбора трафика:  
```
R15#sh access-list R15_to_R18
Extended IP access list R15_to_R18
    10 permit ip 10.1.0.0 0.0.255.255 10.2.0.0 0.0.255.255
```
Определим набор преобразований:  
```
R15#sh run | s crypto ipsec trans
crypto ipsec transform-set IPSEC_TS esp-aes esp-md5-hmac
 mode tunnel
```
И настроим вторую фазу:  
```
R15#sh run | s crypto map
crypto map IPSEC 1 ipsec-isakmp
 set peer 50.50.1.10
 set transform-set IPSEC_TS
 set pfs group5
 set ikev2-profile PROF1
 match address R15_to_R18
```
Настройки IPsec для R18 аналогичны.


