---
## Front matter
title: "Отчёт по лабораторной работе 8"
subtitle: "Настройка сетевых сервисов. DHCP"
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

Приобретение практических навыков по настройке динамического распределения IP-адресов посредством протокола DHCP (Dynamic Host Configuration Protocol) в локальной сети.

# Процесс работы

## Добавление и подключение DNS-сервера

В логической рабочей области Cisco Packet Tracer в существующую топологию сети был добавлен сервер DNS. Сервер был подключён к коммутатору msk-donskaya-dgavadadev-sw-3 через интерфейс FastEthernet0/2.

После подключения порт на коммутаторе был активирован.

В конфигурации сервера был задан статический IP-адрес и параметры сети:
- IP-адрес: 10.128.0.5  
- Маска подсети: 255.255.255.0  
- Шлюз по умолчанию: 10.128.0.1  

![Топология сети с DNS-сервером](Screenshot_1.png){ #fig:001 width=80% }

## Настройка DNS-сервера

На сервере была выполнена настройка службы DNS. В разделе Services была выбрана служба DNS и переведена в состояние On.

После этого были добавлены ресурсные записи типа A (A Record) для сопоставления доменных имён с IP-адресами серверов сети.

Были созданы следующие записи:
- dns.donskaya.rudn.edu → 10.128.0.5  
- file.donskaya.rudn.edu → 10.128.0.3  
- mail.donskaya.rudn.edu → 10.128.0.4  
- www.donskaya.rudn.edu → 10.128.0.2  

После добавления записей конфигурация была сохранена.

![Настройка IP DNS-сервера](Screenshot_2.png){ #fig:002 width=80% }

![Настройка службы DNS](Screenshot_3.png){ #fig:003 width=80% }

## Настройка DHCP на маршрутизаторе

На маршрутизаторе msk-donskaya-dgavadadev-gw-1 была выполнена настройка службы DHCP для автоматической выдачи IP-адресов в различных подсетях.

Сначала был указан адрес DNS-сервера и включена служба DHCP:

enable  
configure terminal  
ip name-server 10.128.0.5  
service dhcp  

Далее были созданы пулы адресов для различных подразделений сети.

### Настройка пула dk

ip dhcp pool dk  
network 10.128.3.0 255.255.255.0  
default-router 10.128.3.1  
dns-server 10.128.0.5  
exit  
ip dhcp excluded-address 10.128.3.1 10.128.3.29  
ip dhcp excluded-address 10.128.3.200 10.128.3.254  

### Настройка пула departments

ip dhcp pool departments  
network 10.128.4.0 255.255.255.0  
default-router 10.128.4.1  
dns-server 10.128.0.5  
exit  
ip dhcp excluded-address 10.128.4.1 10.128.4.29  
ip dhcp excluded-address 10.128.4.200 10.128.4.254  

### Настройка пула adm

ip dhcp pool adm  
network 10.128.5.0 255.255.255.0  
default-router 10.128.5.1  
dns-server 10.128.0.5  
exit  
ip dhcp excluded-address 10.128.5.1 10.128.5.29  
ip dhcp excluded-address 10.128.5.200 10.128.5.254  

### Настройка пула other

ip dhcp pool other  
network 10.128.6.0 255.255.255.0  
default-router 10.128.6.1  
dns-server 10.128.0.5  
exit  
ip dhcp excluded-address 10.128.6.1 10.128.6.29  
ip dhcp excluded-address 10.128.6.200 10.128.6.254  

После завершения настройки конфигурация была сохранена.

![Настройка DHCP на маршрутизаторе](Screenshot_4.png){ #fig:004 width=80% }

## Проверка работы DHCP

Для проверки работы DHCP были использованы команды:

show ip dhcp pool  
show ip dhcp binding  

В результате было установлено, что пулы адресов активны, а IP-адреса успешно выдаются клиентским устройствам.

![Информация о пулах DHCP](Screenshot_7.png){ #fig:005 width=80% }

![Таблица выданных адресов](Screenshot_8.png){ #fig:006 width=80% }

## Настройка клиентских устройств

На оконечных устройствах была выполнена настройка получения IP-адреса по DHCP.

После переключения на динамическую настройку устройства автоматически получили IP-адрес, маску подсети, шлюз и DNS-сервер.

Пример полученных параметров:
- IP-адрес: 10.128.4.30  
- Шлюз: 10.128.4.1  
- DNS: 10.128.0.5  

![Получение IP-адреса по DHCP](Screenshot_5.png){ #fig:007 width=80% }

## Проверка связности сети

Для проверки доступности узлов была выполнена команда ping между устройствами из разных подсетей.

В результате обмен пакетами прошёл успешно, что подтверждает корректную настройку маршрутизации и DHCP.

![Проверка связности сети](Screenshot_6.png){ #fig:008 width=80% }

## Анализ работы DHCP в режиме симуляции

В режиме симуляции Cisco Packet Tracer был изучен процесс получения IP-адреса по протоколу DHCP. В ходе анализа было установлено, что взаимодействие между клиентом и сервером происходит в несколько этапов.

На первом этапе клиентское устройство отправляет широковещательное сообщение DHCP Discover. В данном пакете в поле Client Address указан адрес 0.0.0.0, что означает отсутствие у клиента назначенного IP-адреса. Также передаётся MAC-адрес устройства.

![DHCP Discover](Screenshot_9.png){ #fig:009 width=80% }

На втором этапе DHCP-сервер отвечает сообщением DHCP Offer. В данном сообщении сервер предлагает клиенту IP-адрес из доступного пула. В поле Your Client Address указывается выделяемый адрес, например 10.128.4.30, а также передаётся адрес сервера и дополнительные параметры, включая DNS-сервер.

![DHCP Offer](Screenshot_10.png){ #fig:010 width=80% }

После получения предложения клиент отправляет сообщение DHCP Request, в котором подтверждает согласие использовать предложенный IP-адрес.

![DHCP Request](Screenshot_11.png){ #fig:011 width=80% }

На заключительном этапе сервер отправляет сообщение DHCP Acknowledgment (ACK), подтверждая закрепление IP-адреса за клиентом. В этом сообщении также передаются параметры сети, такие как адрес шлюза и DNS-сервера.

![DHCP ACK](Screenshot_12.png){ #fig:012 width=80% }

# Итоги

## Вывод  

В результате выполнения работы был добавлен и настроен DNS-сервер, а также реализована служба динамической раздачи IP-адресов DHCP на маршрутизаторе. Были созданы пулы адресов для различных подсетей, настроены параметры шлюза и DNS-сервера.

Проверка показала, что клиентские устройства успешно получают IP-адреса автоматически, а также имеют доступ к ресурсам сети. Анализ работы DHCP в режиме симуляции подтвердил корректность обмена сообщениями между клиентом и сервером.

## Контрольные вопросы

**1. За что отвечает протокол DHCP?**  
Протокол DHCP (Dynamic Host Configuration Protocol) отвечает за автоматическую настройку сетевых параметров на клиентских устройствах. Он позволяет динамически назначать IP-адреса, маску подсети, шлюз по умолчанию и другие параметры, что упрощает администрирование сети и снижает вероятность ошибок при ручной настройке.

**2. Какие типы DHCP-сообщений передаются по сети?**  
Основные типы сообщений DHCP:
- DHCP Discover — запрос клиента на получение IP-адреса  
- DHCP Offer — предложение IP-адреса от сервера  
- DHCP Request — подтверждение выбора адреса клиентом  
- DHCP ACK — подтверждение назначения адреса сервером  
Дополнительно могут использоваться DHCP NAK, DHCP Release и DHCP Inform.

**3. Какие параметры могут быть переданы в сообщениях DHCP?**  
В DHCP-сообщениях могут передаваться различные параметры:
- IP-адрес клиента  
- маска подсети  
- шлюз по умолчанию  
- адрес DNS-сервера  
- доменное имя  
- время аренды (lease time)  
- адрес DHCP-сервера  
- дополнительные сетевые параметры (например, NTP-сервер)

**4. Что такое DNS?**  
DNS (Domain Name System) — это система доменных имён, предназначенная для преобразования доменных имён (например, www.donskaya.rudn.edu) в IP-адреса. Это позволяет пользователям обращаться к ресурсам сети по удобным именам вместо числовых адресов.

**5. Какие типы записи описания ресурсов есть в DNS и для чего они используются?**  
Основные типы DNS-записей:
- A — сопоставляет доменное имя с IPv4-адресом  
- AAAA — сопоставляет доменное имя с IPv6-адресом  
- CNAME — задаёт псевдоним (алиас) для другого доменного имени  
- MX — указывает почтовый сервер для домена  
- NS — определяет серверы имён для домена  
- PTR — используется для обратного разрешения (IP → имя)  
- TXT — содержит произвольную текстовую информацию (например, для SPF или проверки домена)