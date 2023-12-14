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

----------------------
| adapter 1 -- ens192 |

| adapter 2 -- ens224 |

| adapter 3 -- ens256 |

| adapter 4 -- ens161 |


![image](https://github.com/goibova5/Alt_Linux/assets/148867942/57fdbc50-d41e-4d29-9aa3-dc55b752d298)


Таблица №1


|Имя устройства |Интерфейс|    IPv4      |Маска/префикс |    Шлюз     |
|---------------|---------|--------------|--------------|-------------|
|               |  ens161 |10.12.15.20   |  /24         |10.12.13.254 |
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



a)
## Способ, если интерфейсы были добавлены до полной установки системы

Для начала посмторим, какие интерфейсы есть на ```ISP```:
```
ip a
```
После открываем файл ```options```  для нужного интерфейса:
```
vim /etc/net/ifaces/ens192/options
```
Там нужно поменять все как на примере:
```
BOOTPROTO=static
TYPE=eth
CONFIG_WIRELESS=no
SYSTEMD_BOOTPROTO=static
CONFIG_IPV4=yes
DISABLED=no
NM_CONTROLLED=no
SYSTEMD_CONTROLLED=no
```
Затем задаем неужный адрем на интерфейс:
```
echo 192.168.0.170/30 > /etc/net/ifaces/ens192/ipv4address
```
Далее добавляем шлюз по умолчанию:
```
echo default via 192.168.0.170 > /etc/net/ifaces/xxx/ipv4route
```
Для указание информации о DNS-сервере, прописываем команду:
```
echo nameserver 8.8.8.8 > /etc/resolv.conf
```
Затем перезагружаем сетевую службу:
```
service network restart
```
И на последок проверяем репзультат:
```
ip a
```
## Способ, если интерфейсы были добавлены после установки системы:

Для начало создаем папку с нужным интерфейсом:
```
mkdir /etc/net/ifaces/ens192
```
Далее в данной папке нужно создать файл ```options``` с параметрами:
```
BOOTPROTO=static
TYPE=eth
CONFIG_WIRELESS=no
SYSTEMD_BOOTPROTO=dhcp4
CONFIG_IPV4=yes
DISABLED=no
NM_CONTROLLED=no
SYSTEMD_CONTROLLED=no
```
После этого задается нужный адрес на интерфейс:
```
echo 192.168.0.170/30 > /etc/net/ifaces/ens192/ipv4address
```
Чтоб добавить шлюз по умолчанию нужна команда:
```
echo default via 192.168.0.170 > /etc/net/ifaces/xxx/ipv4route
```
Для указание информации о DNS-сервере, прописываем команду:
```
echo nameserver 8.8.8.8 > /etc/resolv.conf
```
Затем перезагружаем сетевую службу:
```
service network restart
```
И на последок проверяем репзультат:
```
ip a
```


## NAT с помощью firewalld ISP,HQ-R,BR-R:

Настройки options у интерфейсов:
```
NM_CONTROLLED=no
DISABLED=no
```
Далее установим Firewalld:
```
apt-get -y install firewalld
```
После этого включаем автозагрузку:
```
systemctl enable --now firewalld
```
К исходящим пакетам добавляем правила:
```
firewall-cmd --permanent --zone=public --add-interface=ens33
```
И к входящим пакетам:
```
firewall-cmd --permanent --zone=trusted --add-interface=ens34
```
Включаем NAT:
```
firewall-cmd --permanent --zone=public --add-masquerade
```
Сохраняем правила:
```
firewall-cmd --reload
```

Если Firewalld после перезагрузки машины не загружает введёные команды автоматически, а только после команды:
```
firewall-cmd --reload
```
То стоит проверить через ```vim``` options у интерфейсов, и при необходимости выключить NetworkManager:
```
NM_CONTROLLED=no
```

Так же может помочь команда:
```
systemctl disable network.service NetworkManager
```
 ## Все тоде самое сделать на ISP,HQ-R,BR-R!



https://github.com/Clover136/demo2024/blob/main/ALT%20LINUX.md
