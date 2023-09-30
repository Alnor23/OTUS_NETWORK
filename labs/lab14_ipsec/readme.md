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
В роли СA будет выступать R24 (выполним на нем необходимые настройки):  
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
```
```
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
Далее создадим ACL для отбора трафика идущему через GRE:  
```
R15#sh access-lists GRE1
Extended IP access list GRE1
    10 permit gre any any
```
Определим набор преобразований:  
```
R15#sh run | s crypto ipsec trans
crypto ipsec transform-set IPSEC_TS esp-aes esp-md5-hmac
 mode transport
```
И настроим вторую фазу:  
```
R15#sh run | s crypto map
crypto map IPSEC 1 ipsec-isakmp
 set peer 50.50.1.10
 set transform-set IPSEC_TS
 set pfs group5
 set ikev2-profile PROF1
 match address GRE1
```
Далее применяем crypto map к исходящему интерфейсу:  
```
R15(config)#int e0/2
R15(config-if)#crypto map IPSEC
```

Настройки IPsec для R18 аналогичны.  
Выполним проверку:  
```
R15#sho crypto ikev2 session
 IPv4 Crypto IKEv2 Session

Session-id:2, Status:UP-ACTIVE, IKE count:1, CHILD count:1

Tunnel-id Local                 Remote                fvrf/ivrf            Status
2         50.50.1.38/500        50.50.1.10/500        none/none            READY
      Encr: AES-CBC, keysize: 128, PRF: MD5, Hash: MD596, DH Grp:2, Auth sign: RSA, Auth verify: RSA
      Life/Active Time: 86400/679 sec
Child sa: local selector  0.0.0.0/0 - 255.255.255.255/65535
          remote selector 0.0.0.0/0 - 255.255.255.255/65535
          ESP spi in/out: 0xA6FDEB3/0x25DA684

 IPv6 Crypto IKEv2 Session
```
```
R15#sh crypto ipsec sa

interface: Ethernet0/2
    Crypto map tag: IPSEC, local addr 50.50.1.38

   protected vrf: (none)
   local  ident (addr/mask/prot/port): (0.0.0.0/0.0.0.0/47/0)
   remote ident (addr/mask/prot/port): (0.0.0.0/0.0.0.0/47/0)
   current_peer 50.50.1.10 port 500
     PERMIT, flags={origin_is_acl,}
    #pkts encaps: 233, #pkts encrypt: 233, #pkts digest: 233
    #pkts decaps: 117, #pkts decrypt: 117, #pkts verify: 117
    #pkts compressed: 0, #pkts decompressed: 0
    #pkts not compressed: 0, #pkts compr. failed: 0
    #pkts not decompressed: 0, #pkts decompress failed: 0
    #send errors 0, #recv errors 0

     local crypto endpt.: 50.50.1.38, remote crypto endpt.: 50.50.1.10
     plaintext mtu 1438, path mtu 1500, ip mtu 1500, ip mtu idb Ethernet0/2
     current outbound spi: 0x41DD788(69064584)
     PFS (Y/N): N, DH group: none

     inbound esp sas:
      spi: 0x4A0C4498(1242317976)
        transform: esp-aes esp-md5-hmac ,
        in use settings ={Tunnel, }
        conn id: 2, flow_id: SW:2, sibling_flags 80000040, crypto map: IPSEC
        sa timing: remaining key lifetime (k/sec): (4303392/3255)
        IV size: 16 bytes
        replay detection support: Y
        Status: ACTIVE(ACTIVE)

     inbound ah sas:

     inbound pcp sas:

     outbound esp sas:
      spi: 0x41DD788(69064584)
        transform: esp-aes esp-md5-hmac ,
        in use settings ={Tunnel, }
        conn id: 1, flow_id: SW:1, sibling_flags 80000040, crypto map: IPSEC
        sa timing: remaining key lifetime (k/sec): (4303382/3255)
        IV size: 16 bytes
        replay detection support: Y
        Status: ACTIVE(ACTIVE)

     outbound ah sas:

     outbound pcp sas:
```
Из проверки видно что сессия установлена пакеты шифруются и расшифровываются.  

### Часть 2. Настройте DMVPN поверх IPSec между Москва и Чокурдах, Лабытнанги.  
В данном задании необходимо настроить IPSec между R14, R27 и R28.  
DMVPN 1 фазы был настроен в предыдущей лабораторной работе:  
R14(Москва) он будет выступать в роли хаба:  
```
R14#sh run int tun 1
Building configuration...

Current configuration : 184 bytes
!
interface Tunnel1
 ip address 10.1.7.2 255.255.255.0
 no ip redirects
 ip nhrp map multicast dynamic
 ip nhrp network-id 1
 tunnel source 50.50.1.30
 tunnel mode gre multipoint
end
```
Затем настроим R27(Лабытнанги), R28(Чокурдах) в роли SPOKE:  
R27:  
```
R27#sh run int tu 1
Building configuration...

Current configuration : 228 bytes
!
interface Tunnel1
 ip address 10.1.7.4 255.255.255.0
 ip nhrp map 50.50.1.30 10.1.7.2
 ip nhrp map multicast 50.50.1.30
 ip nhrp network-id 1
 ip nhrp nhs 10.1.7.2
 tunnel source 50.50.1.26
 tunnel destination 50.50.1.30
end
```
R28:  
```
R28#sh run int tu1
Building configuration...

Current configuration : 228 bytes
!
interface Tunnel1
 ip address 10.1.7.3 255.255.255.0
 ip nhrp map multicast 50.50.1.30
 ip nhrp map 50.50.1.30 10.1.7.2
 ip nhrp network-id 1
 ip nhrp nhs 10.1.7.2
 tunnel source 50.50.1.22
 tunnel destination 50.50.1.30
end
```
Настройка аутентификации клиентов IPsec через сертификаты выполняется по аналогии с предыдущей частью В роли СA будет выступать также R24:  
Пример R14:  
```
R14#sh crypto pki trustpoint VPN
Trustpoint VPN:
    Subject Name:
    cn=CA
          Serial Number (hex): 01
    Certificate configured.
    SCEP URL: http://10.10.1.3:80/cgi-bin


R14#sh crypto pki cer
Certificate
  Status: Available
  Certificate Serial Number (hex): 06
  Certificate Usage: General Purpose
  Issuer:
    cn=CA
  Subject:
    Name: R14
    IP Address: 50.50.1.30
    hostname=R14+ipaddress=50.50.1.30
    cn=R14
    ou=VPN
    o=LAB
    c=RU
  Validity Date:
    start date: 22:46:12 UTC Sep 29 2023
    end   date: 22:46:12 UTC Sep 28 2024
  Associated Trustpoints: VPN
  Storage: nvram:CA#6.cer

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
  Storage: nvram:CA#1CA.cer
```
(Настройки на R27, R28 выполнены аналогично).  

Перейдем к настройке IPsec:
Хаб R14:  
```
R14#sh run | sec crypto ikev2
crypto ikev2 proposal PH1
 encryption aes-cbc-128
 integrity md5
 group 2
crypto ikev2 profile PROF1
 match address local 50.50.1.30
 match identity remote address 0.0.0.0 0.0.0.0
 authentication remote rsa-sig
 authentication local rsa-sig
 pki trustpoint VPN

R14#sh run | s crypto ipsec trans
crypto ipsec transform-set IPSEC_TS esp-aes esp-md5-hmac
 mode transport

R28(config)#crypto ipsec profile IPSEC_P1
R28(ipsec-profile)#set transform-set IPSEC_TS
R14#sh crypto ipsec profile
IPSEC profile IPSEC_P1
        Security association lifetime: 4608000 kilobytes/3600 seconds
        Responder-Only (Y/N): N
        PFS (Y/N): N
        Mixed-mode : Disabled
        Transform sets={
                IPSEC_TS:  { esp-aes esp-md5-hmac  } ,
        }

R14#sh run int tun1
Building configuration...

Current configuration : 246 bytes
!
interface Tunnel1
 ip address 10.1.7.2 255.255.255.0
 no ip redirects
 ip nhrp map multicast dynamic
 ip nhrp network-id 1
 tunnel source 50.50.1.30
 tunnel mode gre multipoint
 tunnel protection ipsec profile IPSEC_P1 ikev2-profile PROF1
end

```
SPOKE R27:  
```
R27#sh run | sec crypto ikev2
crypto ikev2 proposal PH1
 encryption aes-cbc-128
 integrity md5
 group 2
crypto ikev2 policy IKEV2
 proposal PH1
crypto ikev2 profile PROF1
 match address local 50.50.1.26
 match identity remote address 50.50.1.30 255.255.255.255
 authentication remote rsa-sig
 authentication local rsa-sig
 pki trustpoint VPN

R27#sh run | s crypto ipsec trans
crypto ipsec transform-set IPSEC_TS esp-aes esp-md5-hmac
 mode transport

R28(config)#crypto ipsec profile IPSEC_P1
R28(ipsec-profile)#set transform-set IPSEC_TS
R27#sh crypto ipsec profile
IPSEC profile IPSEC_P1
        Security association lifetime: 4608000 kilobytes/3600 seconds
        Responder-Only (Y/N): N
        PFS (Y/N): N
        Mixed-mode : Disabled
        Transform sets={
                IPSEC_TS:  { esp-aes esp-md5-hmac  } ,
        }

R27#sh run int tun1
Building configuration...

Current configuration : 290 bytes
!
interface Tunnel1
 ip address 10.1.7.4 255.255.255.0
 ip nhrp map 50.50.1.30 10.1.7.2
 ip nhrp map multicast 50.50.1.30
 ip nhrp network-id 1
 ip nhrp nhs 10.1.7.2
 tunnel source 50.50.1.26
 tunnel destination 50.50.1.30
 tunnel protection ipsec profile IPSEC_P1 ikev2-profile PROF1
end
```
SPOKE R28:  
```
R28#sh run | sec crypto ikev2
crypto ikev2 proposal PH1
 encryption aes-cbc-128
 integrity md5
 group 2
crypto ikev2 policy IKEV2
 proposal PH1
crypto ikev2 profile PROF1
 match address local 50.50.1.22
 match identity remote address 50.50.1.30 255.255.255.255
 authentication remote rsa-sig
 authentication local rsa-sig
 pki trustpoint VPN


R28#sh run | s crypto ipsec trans
crypto ipsec transform-set IPSEC_TS esp-aes esp-md5-hmac
 mode transport

R28(config)#crypto ipsec profile IPSEC_P1
R28(ipsec-profile)#set transform-set IPSEC_TS
R28#sh crypto ipsec profile
IPSEC profile IPSEC_P1
        Security association lifetime: 4608000 kilobytes/3600 seconds
        Responder-Only (Y/N): N
        PFS (Y/N): N
        Mixed-mode : Disabled
        Transform sets={
                IPSEC_TS:  { esp-aes esp-md5-hmac  } ,
        }

interface Tunnel1
 ip address 10.1.7.3 255.255.255.0
 ip nhrp map multicast 50.50.1.30
 ip nhrp map 50.50.1.30 10.1.7.2
 ip nhrp network-id 1
 ip nhrp nhs 10.1.7.2
 tunnel source 50.50.1.22
 tunnel destination 50.50.1.30
 tunnel protection ipsec profile IPSEC_P1 ikev2-profile PROF1
``` 
В данной части в отличии от предыдущей применена настройка на тунельном интерфейсе, а не через route map к исходящему интерфейсу.  
Выполним проверку на хабе R14:  
```
R14#sho crypto ikev2 session
 IPv4 Crypto IKEv2 Session

Session-id:4, Status:UP-ACTIVE, IKE count:1, CHILD count:1

Tunnel-id Local                 Remote                fvrf/ivrf            Status
2         50.50.1.30/500        50.50.1.22/500        none/none            READY
      Encr: AES-CBC, keysize: 128, PRF: MD5, Hash: MD596, DH Grp:2, Auth sign: RSA, Auth verify: RSA
      Life/Active Time: 86400/646 sec
Child sa: local selector  50.50.1.30/0 - 50.50.1.30/65535
          remote selector 50.50.1.22/0 - 50.50.1.22/65535
          ESP spi in/out: 0x474CA551/0xF7DA871F

Session-id:3, Status:UP-ACTIVE, IKE count:1, CHILD count:1

Tunnel-id Local                 Remote                fvrf/ivrf            Status
1         50.50.1.30/500        50.50.1.26/500        none/none            READY
      Encr: AES-CBC, keysize: 128, PRF: MD5, Hash: MD596, DH Grp:2, Auth sign: RSA, Auth verify: RSA
      Life/Active Time: 86400/1139 sec
Child sa: local selector  50.50.1.30/0 - 50.50.1.30/65535
          remote selector 50.50.1.26/0 - 50.50.1.26/65535
          ESP spi in/out: 0x9448171E/0xBF37498B

 IPv6 Crypto IKEv2 Session
```
```
R14#sh crypto ipsec sa

interface: Tunnel1
    Crypto map tag: Tunnel1-head-0, local addr 50.50.1.30

   protected vrf: (none)
   local  ident (addr/mask/prot/port): (50.50.1.30/255.255.255.255/47/0)
   remote ident (addr/mask/prot/port): (50.50.1.22/255.255.255.255/47/0)
   current_peer 50.50.1.22 port 500
     PERMIT, flags={origin_is_acl,}
    #pkts encaps: 1, #pkts encrypt: 1, #pkts digest: 1
    #pkts decaps: 1, #pkts decrypt: 1, #pkts verify: 1
    #pkts compressed: 0, #pkts decompressed: 0
    #pkts not compressed: 0, #pkts compr. failed: 0
    #pkts not decompressed: 0, #pkts decompress failed: 0
    #send errors 0, #recv errors 0

     local crypto endpt.: 50.50.1.30, remote crypto endpt.: 50.50.1.22
     plaintext mtu 1458, path mtu 1500, ip mtu 1500, ip mtu idb (none)
     current outbound spi: 0xF7DA871F(4158293791)
     PFS (Y/N): N, DH group: none

     inbound esp sas:
      spi: 0x474CA551(1196205393)
        transform: esp-aes esp-md5-hmac ,
        in use settings ={Transport, }
        conn id: 7, flow_id: SW:7, sibling_flags 80000000, crypto map: Tunnel1-head-0
        sa timing: remaining key lifetime (k/sec): (4350560/2815)
        IV size: 16 bytes
        replay detection support: Y
        Status: ACTIVE(ACTIVE)

     inbound ah sas:

     inbound pcp sas:

     outbound esp sas:
      spi: 0xF7DA871F(4158293791)
        transform: esp-aes esp-md5-hmac ,
        in use settings ={Transport, }
        conn id: 8, flow_id: SW:8, sibling_flags 80000000, crypto map: Tunnel1-head-0
        sa timing: remaining key lifetime (k/sec): (4350560/2815)
        IV size: 16 bytes
        replay detection support: Y
        Status: ACTIVE(ACTIVE)

     outbound ah sas:

     outbound pcp sas:

   protected vrf: (none)
   local  ident (addr/mask/prot/port): (50.50.1.30/255.255.255.255/47/0)
   remote ident (addr/mask/prot/port): (50.50.1.26/255.255.255.255/47/0)
   current_peer 50.50.1.26 port 500
     PERMIT, flags={origin_is_acl,}
    #pkts encaps: 1, #pkts encrypt: 1, #pkts digest: 1
    #pkts decaps: 1, #pkts decrypt: 1, #pkts verify: 1
    #pkts compressed: 0, #pkts decompressed: 0
    #pkts not compressed: 0, #pkts compr. failed: 0
    #pkts not decompressed: 0, #pkts decompress failed: 0
    #send errors 0, #recv errors 0

     local crypto endpt.: 50.50.1.30, remote crypto endpt.: 50.50.1.26
     plaintext mtu 1458, path mtu 1500, ip mtu 1500, ip mtu idb (none)
     current outbound spi: 0xBF37498B(3208071563)
     PFS (Y/N): N, DH group: none

     inbound esp sas:
      spi: 0x9448171E(2487752478)
        transform: esp-aes esp-md5-hmac ,
        in use settings ={Transport, }
        conn id: 5, flow_id: SW:5, sibling_flags 80000000, crypto map: Tunnel1-head-0
        sa timing: remaining key lifetime (k/sec): (4151378/2322)
        IV size: 16 bytes
        replay detection support: Y
        Status: ACTIVE(ACTIVE)

     inbound ah sas:

     inbound pcp sas:

     outbound esp sas:
      spi: 0xBF37498B(3208071563)
        transform: esp-aes esp-md5-hmac ,
        in use settings ={Transport, }
        conn id: 6, flow_id: SW:6, sibling_flags 80000000, crypto map: Tunnel1-head-0
        sa timing: remaining key lifetime (k/sec): (4151377/2322)
        IV size: 16 bytes
        replay detection support: Y
        Status: ACTIVE(ACTIVE)

     outbound ah sas:

     outbound pcp sas
```
Из проверки видно что сессия установлена пакеты шифруются и расшифровываются.   
_______
 - [Конфигурации устройств](https://github.com/Alnor23/OTUS_NETWORK/tree/main/labs/lab14_ipsec/config)
