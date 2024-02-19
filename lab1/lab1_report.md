##### University: [ITMO University](https://itmo.ru/ru/)
##### Faculty: [FICT](https://fict.itmo.ru)
##### Course: [ip-telephony](https://itmo-ict-faculty.github.io/ip-telephony/)
##### Year: 2023/2024
##### Group: K34202
##### Author: Sbitnev Aleksandr
##### Lab: Lab1
##### Date of create: 15.02.2024
##### Date of finished: 

***

# Отчёт по лабораторной работе №1 "Базовая настройка ip-телефонов в среде Сisco packet tracer"


## **Цель работы:** 
Изучить рабочую среду Cisco Packet Tracer, ознакомить- ся с интерфейсами основных устройств, типами кабелей, научиться собирать топологию. Изучить построение сети IP-телефонии с помощью маршрутизатора, коммутатора и IP телефонов Cisco 7960 в среде Packet tracer.

## **Ход работы:**
### Часть 1
В Cisco Packet Tracer была собрана схема:

![image](https://github.com/Sbitnev/2023_2024-ip-telephony-k34202-sbitnev_a_s/assets/71010852/908ca202-533c-41c3-835f-bce3abb38b3a)

Каждому PC был задан ip-адрес из пула 192.168.1.10-192.168.1.17/24. 

Пример настройки на PC0:

![image](https://github.com/Sbitnev/2023_2024-ip-telephony-k34202-sbitnev_a_s/assets/71010852/faa8ded8-f5d8-45fb-a1c8-e38d4c893541)

Проверим связанность командой ping:

![image](https://github.com/Sbitnev/2023_2024-ip-telephony-k34202-sbitnev_a_s/assets/71010852/c94b1ebe-8e49-421a-889d-dc1a5834f2b4)


### Часть 2
В Cisco Packet Tracer была собрана схема:

![image](https://github.com/Sbitnev/2023_2024-ip-telephony-k34202-sbitnev_a_s/assets/71010852/5c19cb4a-496b-43f3-aa94-91b41137399d)

Изменим имя маршрутизатора на CMERouter.

![image](https://github.com/Sbitnev/2023_2024-ip-telephony-k34202-sbitnev_a_s/assets/71010852/18f5d677-d978-4f77-b2b4-be7d4386990e)

Настроим маршрутизатор. Настроим интерфейс FastEthernet 0/0:
```
interface FastEthernet0/0
 ip address 192.168.1.1 255.255.255.0
```

Также на маршрутизаторе настроим DHCP-сервер:
```
ip dhcp pool MY-POOL
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
 option 150 ip 192.168.1.1
```

Далее на маршрутизаторе настроим VoIP параметры:
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
```

Настроим vlan на коммутаторе:
```
interface FastEthernet0/1
 switchport mode access
 switchport voice vlan 1
!
interface FastEthernet0/2
 switchport mode access
 switchport voice vlan 1
!
interface FastEthernet0/3
 switchport mode access
 switchport voice vlan 1
```

Телефоны получили ip и номера:
![image](https://github.com/Sbitnev/2023_2024-ip-telephony-k34202-sbitnev_a_s/assets/71010852/67669006-6bc6-4476-a13e-714aa5510e6b)

Проверим звонок между телефонами:

![image](https://github.com/Sbitnev/2023_2024-ip-telephony-k34202-sbitnev_a_s/assets/71010852/fb825aa5-3d04-488d-aa9b-af4eab1feaf5)


## **Вывод:** 

В результате выполнения работы была изучена рабочая среда Cisco Packet Tracer, интерфейс основных устройств, типами кабелей, научились собирать топологию. Изучено построение сети IP-телефонии с помощью маршрутизатора, коммутатора и IP телефонов Cisco 7960 в среде Packet tracer.

