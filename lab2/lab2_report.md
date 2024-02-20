##### University: [ITMO University](https://itmo.ru/ru/)
##### Faculty: [FICT](https://fict.itmo.ru)
##### Course: [ip-telephony](https://itmo-ict-faculty.github.io/ip-telephony/)
##### Year: 2023/2024
##### Group: K34202
##### Author: Sbitnev Aleksandr
##### Lab: Lab2
##### Date of create: 16.02.2024
##### Date of finished: 

***

# Отчёт по лабораторной работе №2 "Конфигурация voip в среде Сisco packet tracer"


## **Цель работы:** 
Изучить построение сети IP-телефонии с помощью маршрутизатора Cisco 2811, коммутатора Cisco catalyst 3560 и IP телефонов Cisco 7960.

## **Ход работы:**
### Часть 1
В Cisco Packet Tracer была собрана схема:

![image](https://github.com/Sbitnev/2023_2024-ip-telephony-k34202-sbitnev_a_s/assets/71010852/1437f067-e72d-4884-9ebd-547e472168c1)

В конфигурационном режиме изменим название маршрутизатора на CMERouter.

![image](https://github.com/Sbitnev/2023_2024-ip-telephony-k34202-sbitnev_a_s/assets/71010852/83a2e84f-fa1c-4b72-a381-b1deef22b383)

На маршрутизаторе был отключен синтаксис ввода слов от DNS серверов:
```
no ip domain-lookup
```

Были заданы пароли для защиты маршрутизатора в удаленном режиме и в режиме консоли:

```
line con 0
 password cisco
 logging synchronous
!
line aux 0
!
line vty 0 4
 password cisco
 logging synchronous
 login
```

Настроим интерфейс fa0/0 на маршрутизаторе Cisco 2811 (CMERouter).

```
interface FastEthernet0/0
 ip address 192.168.1.1 255.255.255.0
```

Настроим DHCP сервера для передачи голоса и данных на маршрутизаторе Cisco 2811.

```
ip dhcp pool MY-POOL
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
 option 150 ip 192.168.1.1
```

Настроим услуги телефонии Cisco CallManager Express на маршрутизаторе 2811.

```
telephony-service
 max-ephones 15
 max-dn 15
 ip source-address 192.168.1.1 port 3100
 auto assign 1 to 19
!
ephone-dn 1
 number 11111
!
ephone-dn 2
 number 2222
!
ephone-dn 3
 number 3333
```

Создадим VLAN порты на коммутаторе Cisco Catalyst 3560 для взаимодействия коммутатора с маршрутизатором и подключить IP телефоны.
```
interface FastEthernet0/1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport voice vlan 1
!
interface FastEthernet0/2
 switchport mode access
 switchport voice vlan 1
!
interface FastEthernet0/3
 switchport mode access
 switchport voice vlan 1
!
interface FastEthernet0/4
 switchport mode access
 switchport voice vlan 1
```

Проверим звонки между телефонами и проверить остальные сервисы.

![image](https://github.com/Sbitnev/2023_2024-ip-telephony-k34202-sbitnev_a_s/assets/71010852/798c5c8e-1769-4d4e-9584-bb5b06f8a80f)

![image](https://github.com/Sbitnev/2023_2024-ip-telephony-k34202-sbitnev_a_s/assets/71010852/4492599d-1fc3-40ef-b681-28e7b2957c4a)


### Часть 2
В Cisco Packet Tracer была собрана схема:

![image](https://github.com/Sbitnev/2023_2024-ip-telephony-k34202-sbitnev_a_s/assets/71010852/deeaaa67-4998-40fa-aefd-bcfd835c0a84)

Создадим 2 VLAN порта на коммутаторе для взаимодействия коммутатора с маршрутизатором.

```
vlan 10
name data
vlan 20
name voice
```

Настроим интерфейсы коммутатора:

```
interface FastEthernet0/1
 switchport trunk native vlan 30
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport voice vlan 1
!
interface FastEthernet0/2
 switchport access vlan 10
 switchport mode access
 switchport voice vlan 20
!
interface FastEthernet0/3
 switchport access vlan 10
 switchport mode access
 switchport voice vlan 20
!
interface FastEthernet0/4
 switchport access vlan 10
 switchport mode access
 switchport voice vlan 20
```

Зададим маршрут по умолчанию командой ip default-gateway.

```
ip default-gateway 192.168.30.1
```

Настроим DHCP сервера для передачи голоса и данных на маршрутизаторе Cisco 2811 и интерфейсы.

```
ip dhcp excluded-address 192.168.10.1 192.168.10.10
ip dhcp excluded-address 192.168.20.1 192.168.20.10
!
ip dhcp pool DATA
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
ip dhcp pool VOICE
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
 option 150 ip 192.168.20.1
!
interface FastEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
!
interface FastEthernet0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
```

Настроим услуги телефонии Cisco CallManager Express на маршрутизаторе.

```
telephony-service
 max-ephones 3
 max-dn 3
 ip source-address 192.168.20.1 port 3100
 auto assign 1 to 3
!
ephone-dn 1
 number 1010
!
ephone-dn 2
 number 2020
!
ephone-dn 3
 number 3030
```

На компьютерах включим получение ip-адреса по DHCP.

![image](https://github.com/Sbitnev/2023_2024-ip-telephony-k34202-sbitnev_a_s/assets/71010852/a5bf55c7-db28-464e-a5d3-e43142d14a3c)

Проверим звонки между телефонами и пинги.

![image](https://github.com/Sbitnev/2023_2024-ip-telephony-k34202-sbitnev_a_s/assets/71010852/e220c141-769b-410c-a044-7e36f70e5b9a)


![image](https://github.com/Sbitnev/2023_2024-ip-telephony-k34202-sbitnev_a_s/assets/71010852/b9f72930-499e-4868-9580-71c07af6a21a)


## **Вывод:** 

В результате выполнения работы были изучено построение сети IP-телефонии с помощью маршрутизатора Cisco 2811, коммутатора Cisco catalyst 3560 и IP телефонов Cisco 7960. 
