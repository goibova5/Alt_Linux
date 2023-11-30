## Alt_Linux


# Задание 1.1
1. Выполните базовую настройку всех устройств:
2. 
	a. Соберите топологию согласно рисунку. Все устройства работают на OC Linux – ALT

· ISP - Альт Сервер 10.2 (CLI)

· CLI - Альт Рабочая станция 10.2 (GUI)

· HQ-R - Альт Сервер 10.2 (CLI)

· HQ-SRV - Альт Сервер 10.2 (GUI)

· BR-R - Альт Сервер 10.2 (CLI)

· BR-SRV - Альт Сервер 10.2 (CLI)

b. Присвоить имена в соответствии с топологией

c. Рассчитайте IP-адресацию IPv4. Необходимо заполнить таблицу №1. При необходимости отредактируйте таблицу.

d. Пул адресов для сети офиса BRANCH - не более 16. Для IPv6 пропустите этот пункт.

e. Пул адресов для сети офиса HQ - не более 64.




![image](https://github.com/goibova5/Alt_Linux/assets/148867942/57fdbc50-d41e-4d29-9aa3-dc55b752d298)


Таблица №1


|Имя устройства |Интерфейс|    IPv4      |Маска/префикс |    Шлюз     |
|---------------|---------|--------------|--------------|-------------|
|               |  ens161 |10.12.13.88   |  /24         |10.12.13.254 |
|   ISP         |  ens224 |192.168.0.162 |  /30         |             |                              
|               |  ens256 |192.168.0.166 |  /30         |             |
|               |  ens192 |192.168.0.170 |  /30         |             |
|   HQ-R        |  ens192 |192.168.0.1   |  /25         |             |
|               |  ens224 |192.168.0.161 |  /30         |192.168.0.162|
|   BR-R        |  ens192 |192.168.0.129 |  /27         |             |
|               |  ens224 |192.168.0.165 |  /30         |192.168.0.166|
|   HQ-SRV      |  ens192 |192.168.0.2   |  /25         |192.168.0.1  |
|   BR-SRV      |  ens192 |192.168.0.130 |  /27         |192.168.0.129|
|   CLI         |  ens192 |192.168.0.129 |  /30         |192.168.0.170|