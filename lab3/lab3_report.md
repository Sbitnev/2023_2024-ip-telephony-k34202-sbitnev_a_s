##### University: [ITMO University](https://itmo.ru/ru/)
##### Faculty: [FICT](https://fict.itmo.ru)
##### Course: [ip-telephony](https://itmo-ict-faculty.github.io/ip-telephony/)
##### Year: 2023/2024
##### Group: K34202
##### Author: Sbitnev Aleksandr
##### Lab: Lab2
##### Date of create: 25.02.2024
##### Date of finished: 

***

# Отчёт по лабораторной работе №3 "Использование Asterisk в качестве SIP proxy"


## **Цель работы:** 
Изучить программный комплекс Asterisk. Настройка Asterisk для локальных звонков.

## **Ход работы:**
Работа будет выполняться на витруальной машине на Ubuntu.

### Установика Asterisk.
На ВМ был установлен Asterisk командой 
```
sudo apt-get install asterisk
```

![image](https://github.com/Sbitnev/2023_2024-ip-telephony-k34202-sbitnev_a_s/assets/71010852/249f5cc2-7287-455d-a1c6-dfdfa9ef9b46)

### Настрйка SIP каналов.
Далее в директории /etc/asterisk был открыт файл sip.conf, куда была добавлена информация о телефонах 1000 и 1001:
```
[1000]
type=friend
host=dynamic
secret=1000
context=ext_1000

[1001]
type=friend
host=dynamic
secret=1001
context=ext_1001
```

В файл extensions.conf был определен тип канала – SIP:
```
[ext_1000]
exten => _XXXX,1,Dial(SIP/${EXTEN})

[ext_1001]
exten => _XXXX,1,Dial(SIP/${EXTEN})
```
После измененя файлов Asterisk был перезапущен:
```
sudo service asterisk restart
```

Проверим его статус:
```
sudo systemctl status asterisk
```

![image](https://github.com/Sbitnev/2023_2024-ip-telephony-k34202-sbitnev_a_s/assets/71010852/fbbbfbee-8345-4b9e-8565-da8832651bb6)


### Установика soft телефонов на рабочую станцию и подключение к SIP каналам soft телефона..
В качестве soft телефона был выбран Zoiper5. Переходим на сайт разработчика, качаем архив для Linux и запускаем программу.

Зайдем под именем телефона 1000:

![image](https://github.com/Sbitnev/2023_2024-ip-telephony-k34202-sbitnev_a_s/assets/71010852/163bc31f-04e5-4365-a2ba-6f286e4f30e6)

В качестве хоста указваем локалхост.

Проверим подключение:
```
sudo asterisk
sip show peers
```

![image](https://github.com/Sbitnev/2023_2024-ip-telephony-k34202-sbitnev_a_s/assets/71010852/0aa6b9d8-0774-4820-bee8-9fa83c676c11)


### Сделать тестовый звонок на номер 1000
Наберем номер 1000 в ГИ телефона:

![image](https://github.com/Sbitnev/2023_2024-ip-telephony-k34202-sbitnev_a_s/assets/71010852/87eb1d98-79a0-4fcd-a6a3-1b8494c02143)

Звонок проходит, следовательно телефон настроен правильно.

## **Вывод:** 

В результате выполнения работы были изучены програмный комплекс Asterisk и настройка Asterisk для локальных звонков.
