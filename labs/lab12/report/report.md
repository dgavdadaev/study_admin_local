---
## Front matter
title: "Отчёт по лабораторной работе 12"
subtitle: "Настройка NAT"
author: "Авдадаев Джамал Геланиевич"

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true # Table of contents
toc-depth: 2
lof: true # List of figures
lot: true # List of tables
fontsize: 12pt
linestretch: 1.5
papersize: a
documentclass: scrreprt
## I18n polyglossia
polyglossia-lang:
  name: russian
  options:
	- spelling=modern
	- babelshorthands=true
polyglossia-otherlangs:
  name: english
## I18n babel
babel-lang: russian
babel-otherlangs: english
## Fonts
mainfont: IBM Plex Serif
romanfont: IBM Plex Serif
sansfont: IBM Plex Sans
monofont: IBM Plex Mono
mathfont: STIX Two Math
mainfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
romanfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
sansfontoptions: Ligatures=Common,Ligatures=TeX,Scale=MatchLowercase,Scale=0.94
monofontoptions: Scale=MatchLowercase,Scale=0.94,FakeStretch=0.9
mathfontoptions:
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Pandoc-crossref LaTeX customization
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lofTitle: "Список иллюстраций"
lotTitle: "Список таблиц"
lolTitle: "Листинги"
## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Введение

## Цель работы  

Приобретение практических навыков по настройке доступа локальной сети к внешней сети посредством NAT.

# Процесс работы

## Настройка доступа к сети провайдера и NAT

В Cisco Packet Tracer была продолжена настройка сети согласно заданию.  
В топологию были включены маршрутизатор провайдера `provider-dgavadadaev-gw-1`, коммутатор провайдера `provider-dgavadadaev-sw-1`, а также маршрутизатор сети «Донская» `msk-donskaya-dgavadadaev-gw-1`.

В результате была подготовлена схема подключения локальной сети организации к сети провайдера с последующей настройкой NAT и доступа к внутренним сервисам из внешней сети.

![Топология сети](Screenshot_1.png){ #fig:001 width=80% }

## Первоначальная настройка маршрутизатора provider-dgavadadaev-gw-1

На маршрутизаторе провайдера была выполнена базовая настройка. Устройству было присвоено имя `provider-dgavadadaev-gw-1`, настроены пароли доступа через консоль и виртуальные терминалы, включено шифрование паролей, а также создан пользователь администратора.

enable  
configure terminal  
hostname provider-dgavadadaev-gw-1  

line vty 0 4  
password cisco  
login  
exit  

line console 0  
password cisco  
login  
exit  

enable secret cisco  
service password-encryption  
username admin privilege 1 secret cisco  

После этого были включены интерфейсы маршрутизатора. Для подключения к сети «Донская» был настроен подинтерфейс `FastEthernet0/0.4` с инкапсуляцией `dot1Q 4` и IP-адресом из внешней сети.

interface FastEthernet0/0  
no shutdown  
exit  

interface FastEthernet0/0.4  
encapsulation dot1Q 4  
ip address 198.51.100.1 255.255.255.240  
description msk-donskaya  
exit  

interface FastEthernet0/1  
no shutdown  
ip address 192.0.2.1 255.255.255.0  
description internet  
exit  

![Настройка маршрутизатора провайдера](Screenshot_2.png){ #fig:002 width=80% }

## Первоначальная настройка коммутатора provider-dgavadadaev-sw-1

На коммутаторе провайдера была выполнена базовая конфигурация. Было задано имя устройства, настроены пароли для доступа через консоль и VTY-линии, включено шифрование паролей, задан пароль привилегированного режима и создан пользователь администратора.

enable  
configure terminal  
hostname provider-dgavadadaev-sw-1  

line vty 0 4  
password cisco  
login  
exit  

line console 0  
password cisco  
login  
exit  

enable secret cisco  
service password-encryption  
username admin privilege 1 secret cisco  

Также были настроены интерфейсы коммутатора. Порты `FastEthernet0/1` и `FastEthernet0/24` переведены в режим trunk для передачи VLAN между маршрутизаторами и коммутатором провайдера.

interface FastEthernet0/1  
switchport mode trunk  
exit  

interface FastEthernet0/24  
switchport mode trunk  
exit  

После этого была создана VLAN 4 с именем `nat`, а интерфейс VLAN был включен.

vlan 4  
name nat  
exit  

interface vlan 4  
no shutdown  
exit  

![Настройка коммутатора провайдера](Screenshot_3.png){ #fig:003 width=80% }

## Настройка интерфейсов маршрутизатора сети «Донская»

На маршрутизаторе `msk-donskaya-dgavadadaev-gw-1` был настроен интерфейс для связи с сетью провайдера. Физический интерфейс `FastEthernet0/1` был включен, после чего был создан подинтерфейс `FastEthernet0/1.4`.

Для подинтерфейса была настроена инкапсуляция `dot1Q 4`, назначен IP-адрес `198.51.100.2/28` и добавлено описание интерфейса.

configure terminal  

interface FastEthernet0/1  
no shutdown  
exit  

interface FastEthernet0/1.4  
encapsulation dot1Q 4  
ip address 198.51.100.2 255.255.255.240  
description internet  
exit  

Также был настроен маршрут по умолчанию через маршрутизатор провайдера.

ip route 0.0.0.0 0.0.0.0 198.51.100.1  

![Настройка подключения маршрутизатора сети «Донская» к провайдеру](Screenshot_4.png){ #fig:004 width=80% }

## Настройка NAT на маршрутизаторе сети «Донская»

Для выхода внутренних узлов локальной сети во внешнюю сеть был настроен динамический NAT с перегрузкой. Сначала был создан пул внешних адресов `main-pool`.

ip nat pool main-pool 198.51.100.2 198.51.100.14 netmask 255.255.255.240  

Затем был создан расширенный список доступа `nat-inet`, определяющий разрешенный трафик из внутренних сетей.

ip access-list extended nat-inet  
remark dk  
permit tcp 10.128.3.0 0.0.0.255 host 192.0.2.11 eq 80  
permit tcp 10.128.3.0 0.0.0.255 host 192.0.2.12 eq 80  
permit tcp 10.128.4.0 0.0.0.255 host 192.0.2.13 eq 80  
permit tcp 10.128.5.0 0.0.0.255 host 192.0.2.14 eq 80  
permit ip host 10.128.6.200 any  
permit ip host 10.128.6.222 any  
exit  

После создания списка доступа была включена трансляция адресов с использованием пула `main-pool` и параметра `overload`.

ip nat inside source list nat-inet pool main-pool overload  

Внутренние подинтерфейсы маршрутизатора были обозначены как `ip nat inside`, а внешний подинтерфейс `FastEthernet0/1.4` — как `ip nat outside`.

interface FastEthernet0/0.3  
ip nat inside  
exit  

interface FastEthernet0/0.101  
ip nat inside  
exit  

interface FastEthernet0/0.102  
ip nat inside  
exit  

interface FastEthernet0/0.103  
ip nat inside  
exit  

interface FastEthernet0/0.104  
ip nat inside  
exit  

interface FastEthernet0/1.4  
ip nat outside  
exit  

## Настройка доступа из внешней сети во внутреннюю сеть

Для обеспечения доступа из внешней сети к внутренним сервисам организации были настроены статические NAT-трансляции портов.

Были опубликованы внутренние серверы с использованием внешних адресов из сети `198.51.100.0/28`. Для веб-серверов были настроены пробросы портов HTTP, для FTP-сервера — порты 20 и 21, для почтового сервера — порты 25 и 110, а также был настроен доступ по RDP к внутреннему узлу.

ip nat inside source static tcp 10.128.0.2 80 198.51.100.2 80  

ip nat inside source static tcp 10.128.0.3 20 198.51.100.3 20  
ip nat inside source static tcp 10.128.0.3 21 198.51.100.3 21  

ip nat inside source static tcp 10.128.0.4 25 198.51.100.4 25  
ip nat inside source static tcp 10.128.0.4 110 198.51.100.4 110  

ip nat inside source static tcp 10.128.6.200 3389 198.51.100.10 3389  

После завершения настройки конфигурация маршрутизатора была сохранена в память устройства.

end  
write memory  

![Настройка NAT и статических трансляций](Screenshot_5.png){ #fig:005 width=80% }

## Проверка работоспособности настроек

После завершения настройки оборудования была выполнена проверка доступа пользователей различных сегментов сети к внешним ресурсам в соответствии с заданными правилами.

### Проверка доступа из сети дисплейных классов

С компьютера сети дисплейных классов был выполнен доступ к разрешённому сайту www.yandex.ru. Соединение установлено успешно, страница загрузилась корректно.

![Доступ к www.yandex.ru](Screenshot_6.png){ #fig:006 width=80% }

Попытка доступа к запрещённому ресурсу esystem.pfur.ru завершилась ошибкой, что подтверждает корректную работу ограничений.

![Запрещённый доступ к esystem.pfur.ru](Screenshot_7.png){ #fig:007 width=80% }

Также был проверен доступ к образовательному ресурсу stud.rudn.university. Соединение выполнено успешно.

![Доступ к stud.rudn.university](Screenshot_8.png){ #fig:008 width=80% }

### Проверка доступа из сети кафедр

С компьютера сети кафедр был выполнен доступ к образовательному порталу esystem.pfur.ru. Доступ разрешён, страница успешно открыта.

![Доступ к esystem.pfur.ru](Screenshot_9.png){ #fig:009 width=80% }

### Проверка доступа из сети администрации

С компьютера административной сети был выполнен доступ к сайту университета www.rudn.ru. Соединение установлено успешно.

![Доступ к www.rudn.ru](Screenshot_10.png){ #fig:010 width=80% }

### Проверка доступа для прочих пользователей

В сети прочих пользователей доступ в Интернет ограничен. При попытке обращения к внешнему ресурсу www.rudn.ru возникает ошибка разрешения имени, что подтверждает отсутствие доступа.

![Отсутствие доступа у обычного пользователя](Screenshot_11.png){ #fig:011 width=80% }

При этом компьютер администратора в данной сети имеет полный доступ во внешнюю сеть. Проверка показала успешное открытие сайта www.rudn.ru.

![Полный доступ администратора](Screenshot_12.png){ #fig:012 width=80% }

## Проверка доступности серверов из внешней сети

После настройки статического NAT была выполнена проверка доступности внутренних серверов из внешней сети в соответствии с заданными ограничениями по портам.

Для тестирования использовался внешний узел `test-dgavadadaev`, подключённый к сети провайдера.

![Параметры внешнего тестового узла](Screenshot_13.png){ #fig:013 width=80% }

### Проверка доступа к WEB-серверу

С внешнего узла был выполнен доступ к WEB-серверу по IP-адресу 198.51.100.2 с использованием протокола HTTP (порт 80). Страница успешно загрузилась, что подтверждает корректную настройку проброса порта.

![Доступ к WEB-серверу по HTTP](Screenshot_14.png){ #fig:014 width=80% }

### Проверка доступа к FTP-серверу

Для проверки файлового сервера было выполнено подключение по протоколу FTP к адресу 198.51.100.3. Соединение установлено успешно, выполнена авторизация пользователя, что подтверждает доступность портов 20 и 21.

![Подключение к FTP-серверу](Screenshot_15.png){ #fig:015 width=80% }

### Проверка доступа к почтовому серверу

Была выполнена проверка работы почтового сервера через почтовый клиент. Пользователь успешно получил письмо, что подтверждает доступность сервера по портам SMTP (25) и POP3 (110).

![Работа почтового сервера](Screenshot_16.png){ #fig:016 width=80% }

# Итоги

## Вывод  

В результате выполнения работы была настроена сеть с подключением к провайдеру и реализована трансляция сетевых адресов (NAT). Были настроены маршрутизатор и коммутатор провайдера, выполнена настройка интерфейсов и маршрутизации, реализован динамический и статический NAT, а также настроены правила доступа пользователей различных сегментов сети к внешним ресурсам.

Проверка показала корректную работу ограничений доступа и доступность внутренних серверов из внешней сети по заданным портам.

## Контрольные вопросы

**1. В чём состоит основной принцип работы NAT (что даёт наличие NAT в сети организации)?**  
NAT (Network Address Translation) выполняет преобразование внутренних (частных) IP-адресов в внешние (публичные) и обратно. Это позволяет устройствам локальной сети выходить в Интернет, используя один или несколько внешних IP-адресов. Основное преимущество NAT — экономия публичных IP-адресов и повышение безопасности, так как внутренняя структура сети скрыта от внешних пользователей.

**2. В чём состоит принцип настройки NAT (на каком оборудовании и что нужно настроить для выхода из локальной сети во внешнюю сеть через NAT)?**  
NAT настраивается на маршрутизаторе, который соединяет локальную и внешнюю сети. Для его работы необходимо:
- определить внутренние (inside) и внешние (outside) интерфейсы;
- создать список доступа (ACL), определяющий трафик для трансляции;
- при необходимости настроить пул внешних IP-адресов;
- включить правило трансляции (например, с перегрузкой — overload).  
После этого внутренние устройства смогут выходить во внешнюю сеть через маршрутизатор.

**3. Можно ли применить Cisco IOS NAT к субинтерфейсам?**  
Да, NAT можно применять к субинтерфейсам. В Cisco IOS допускается назначение параметров `ip nat inside` и `ip nat outside` как на физические интерфейсы, так и на логические субинтерфейсы, что удобно при использовании VLAN и технологии trunk.

**4. Что такое пулы IP NAT?**  
Пул IP NAT — это диапазон внешних IP-адресов, который используется для динамической трансляции внутренних адресов. При обращении клиентов к внешней сети маршрутизатор выбирает свободный адрес из пула и выполняет преобразование. Это позволяет обслуживать множество пользователей с использованием ограниченного количества внешних адресов.

**5. Что такое статические преобразования NAT?**  
Статический NAT — это фиксированное соответствие между внутренним и внешним IP-адресом или портом. Оно настраивается вручную и используется для публикации внутренних серверов во внешней сети. Например, можно настроить доступ к веб-серверу по порту 80 или к почтовому серверу по портам 25 и 110.