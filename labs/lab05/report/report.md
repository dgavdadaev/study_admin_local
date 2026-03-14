---
## Front matter
title: "Отчёт по лабораторной работе 5"
subtitle: "Конфигурирование VLAN"
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

Получить основные навыки по настройке VLAN на коммутаторах сети.

# Процесс работы

## Построение топологии сети в Cisco Packet Tracer

В логической рабочей области Cisco Packet Tracer была построена топология сети согласно выданной схеме.  
На рабочем поле были размещены коммутаторы серии 2950/2960, пользовательские рабочие станции и серверы.

Каждому сетевому устройству было присвоено имя в соответствии со структурой сети. После размещения устройств они были соединены между собой через интерфейсы FastEthernet и GigabitEthernet.

В результате была сформирована локальная сеть, включающая магистральные соединения между коммутаторами, пользовательские рабочие станции различных подразделений и серверы служб.

![Топология сети в Packet Tracer](Screenshot_1.png){ #fig:001 width=80% }

## Настройка VTP-сервера и создание VLAN на коммутаторе msk-donskaya-dgavadadev-sw-1

На коммутаторе msk-donskaya-dgavadadev-sw-1 была выполнена настройка протокола VTP в режиме сервера.  
Был задан домен VTP, установлен пароль для синхронизации базы VLAN и созданы VLAN согласно структуре сети.

В процессе конфигурации были созданы VLAN для серверов, рабочих станций различных подразделений и административных пользователей.

enable  
configure terminal  
vtp mode server  
vtp domain donskaya  
vtp password cisco  

vlan 2  
name management  

vlan 3  
name servers  

vlan 101  
name dk  

vlan 102  
name departments  

vlan 103  
name adm  

vlan 104  
name other  

После создания VLAN были настроены магистральные соединения между коммутаторами.

interface f0/24  
switchport mode trunk  

interface f0/1  
switchport mode trunk  

interface g0/1  
switchport mode trunk  

interface g0/2  
switchport mode trunk  

![Настройка VTP-сервера и VLAN](Screenshot_2.png){ #fig:002 width=80% }

## Настройка коммутатора msk-donskaya-dgavadadev-sw-2 как VTP-клиента

На коммутаторе msk-donskaya-dgavadadev-sw-2 была выполнена настройка режима VTP-клиента.  
Устройство было подключено к домену VTP и получило информацию о VLAN автоматически от VTP-сервера.

После этого был настроен магистральный канал и назначены VLAN для пользовательских портов.

enable  
configure terminal  

vtp mode client  
vtp domain donskaya  
vtp password cisco  

interface range g0/1-g0/2  
switchport mode trunk  

interface range f0/1-f0/2  
switchport mode access  
switchport access vlan 3  

Конфигурация была сохранена в энергонезависимой памяти устройства.

write memory  

![Настройка коммутатора msk-donskaya-dgavadadev-sw-2](Screenshot_3.png){ #fig:003 width=80% }

## Настройка коммутатора msk-donskaya-dgavadadev-sw-3 как VTP-клиента

На коммутаторе msk-donskaya-dgavadadev-sw-3 также был настроен режим VTP-клиента.  
Коммутатор подключён к домену VTP и автоматически получил список VLAN.

Далее был настроен магистральный порт для соединения с другими коммутаторами и назначены VLAN для портов доступа.

enable  
configure terminal  

vtp mode client  
vtp domain donskaya  
vtp password cisco  

interface g0/1  
switchport mode trunk  

interface range f0/1-f0/2  
switchport mode access  
switchport access vlan 3  

После завершения настройки конфигурация была сохранена.

write memory  

![Настройка коммутатора msk-donskaya-dgavadadev-sw-3](Screenshot_4.png){ #fig:004 width=80% }

## Настройка коммутатора msk-donskaya-dgavadadev-sw-4 как VTP-клиента

Коммутатор msk-donskaya-dgavadadev-sw-4 был настроен в режиме VTP-клиента и подключён к тому же домену VTP. После синхронизации VLAN были выполнены настройки портов доступа для различных подразделений сети.

enable  
configure terminal  

vtp mode client  
vtp domain donskaya  
vtp password cisco  

interface g0/1  
switchport mode trunk  

interface range f0/1-f0/5  
switchport mode access  
switchport access vlan 101  

interface range f0/6-f0/10  
switchport mode access  
switchport access vlan 102  

interface range f0/11-f0/15  
switchport mode access  
switchport access vlan 103  

interface range f0/16-f0/20  
switchport mode access  
switchport access vlan 104  

После завершения настройки конфигурация устройства была сохранена.

write memory  

![Настройка коммутатора msk-donskaya-dgavadadev-sw-4](Screenshot_5.png){ #fig:005 width=80% }

## Настройка коммутатора msk-pavlovskaya-dgavadadev-sw-1

Коммутатор msk-pavlovskaya-dgavadadev-sw-1 был настроен в режиме VTP-клиента. После подключения к домену VTP он автоматически получил информацию о VLAN.

Далее был настроен магистральный порт для соединения с центральным коммутатором и назначены VLAN для пользовательских интерфейсов.

enable  
configure terminal  

vtp mode client  
vtp domain donskaya  
vtp password cisco  

interface f0/24  
switchport mode trunk  

interface range f0/1-f0/15  
switchport mode access  
switchport access vlan 101  

interface f0/20  
switchport mode access  
switchport access vlan 104  

После завершения конфигурации настройки были сохранены.

write memory  

![Настройка коммутатора msk-pavlovskaya-dgavadadev-sw-1](Screenshot_6.png){ #fig:006 width=80% }

## Настройка IP-адресов серверов

На серверах сети были настроены статические IP-адреса в соответствии с таблицей адресации.  
Каждому серверу был назначен уникальный IP-адрес из сети серверного сегмента, а также указан шлюз по умолчанию для обеспечения маршрутизации между VLAN.

На сервере web-dgavadadev был настроен следующий сетевой адрес:

IP Address: 10.128.0.2  
Subnet Mask: 255.255.255.0  
Gateway: 10.128.0.1  

![Настройка IP-адреса сервера web](Screenshot_12.png){ #fig:007 width=80% }

На сервере file-dgavadadev был настроен IP-адрес:

IP Address: 10.128.0.3  
Subnet Mask: 255.255.255.0  
Gateway: 10.128.0.1  

![Настройка IP-адреса сервера file](Screenshot_13.png){ #fig:008 width=80% }

На сервере mail-dgavadadev был настроен IP-адрес:

IP Address: 10.128.0.4  
Subnet Mask: 255.255.255.0  
Gateway: 10.128.0.1  

![Настройка IP-адреса сервера mail](Screenshot_14.png){ #fig:009 width=80% }

## Настройка IP-адресов оконечных устройств

После настройки серверов были настроены сетевые параметры пользовательских рабочих станций.  
Для каждого компьютера был назначен статический IP-адрес из диапазона соответствующей VLAN и указан адрес шлюза по умолчанию.

На рабочей станции dk-donskaya-dgavadadev-1 был настроен следующий IP-адрес:

IP Address: 10.128.3.5  
Subnet Mask: 255.255.255.0  
Gateway: 10.128.3.1  

![Настройка IP-адреса ПК dk-donskaya](Screenshot_7.png){ #fig:010 width=80% }

На рабочей станции dk-pavlovskaya-dgavadadev-1 был настроен IP-адрес:

IP Address: 10.128.3.15  
Subnet Mask: 255.255.255.0  
Gateway: 10.128.3.1  

![Настройка IP-адреса ПК dk-pavlovskaya](Screenshot_8.png){ #fig:011 width=80% }

На рабочей станции dep-donskaya-dgavadadev-1 был настроен следующий адрес:

IP Address: 10.128.4.5  
Subnet Mask: 255.255.255.0  
Gateway: 10.128.4.1  

![Настройка IP-адреса ПК departments](Screenshot_9.png){ #fig:012 width=80% }

На рабочей станции adm-donskaya-dgavadadev-1 был назначен IP-адрес:

IP Address: 10.128.5.5  
Subnet Mask: 255.255.255.0  
Gateway: 10.128.5.1  

![Настройка IP-адреса ПК adm](Screenshot_10.png){ #fig:013 width=80% }

На рабочей станции other-donskaya-dgavadadev-1 был настроен следующий адрес:

IP Address: 10.128.6.5  
Subnet Mask: 255.255.255.0  
Gateway: 10.128.6.1  

![Настройка IP-адреса ПК other](Screenshot_11.png){ #fig:014 width=80% }

На рабочей станции othet-pavlovskaya-dgavadadev-1 был назначен IP-адрес:

IP Address: 10.128.6.15  
Subnet Mask: 255.255.255.0  
Gateway: 10.128.6.1  

![Настройка IP-адреса ПК other pavlovskaya](Screenshot_15.png){ #fig:015 width=80% }

## Проверка сетевой связности с помощью команды ping

После настройки IP-адресов на всех оконечных устройствах была выполнена проверка сетевой связности. Проверка осуществлялась с помощью команды ping из командной строки компьютеров и серверов.

Сначала была проверена доступность устройств, находящихся в одном VLAN. Например, с компьютера other-donskaya-dgavadadev-1 был выполнен ping к устройству с адресом 10.128.6.15. В результате были получены ответы от удалённого узла, что подтверждает корректную работу сети внутри одного VLAN.

C:\>ping 10.128.6.15

Reply from 10.128.6.15: bytes=32 time<1ms TTL=128  
Reply from 10.128.6.15: bytes=32 time<1ms TTL=128  
Reply from 10.128.6.15: bytes=32 time<1ms TTL=128  
Reply from 10.128.6.15: bytes=32 time<1ms TTL=128  

Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)

![Проверка связи внутри VLAN](Screenshot_16.png){ #fig:016 width=80% }

Далее была выполнена проверка связи между устройствами, принадлежащими различным VLAN. Например, с компьютера other-donskaya-dgavadadev-1 был выполнен ping к адресу 10.128.4.5.

C:\>ping 10.128.4.5

Request timed out  
Request timed out  
Request timed out  
Request timed out  

Packets: Sent = 4, Received = 0, Lost = 4 (100% loss)

Отсутствие ответа подтверждает, что устройства, находящиеся в разных VLAN, не имеют прямой сетевой доступности.

![Проверка связи между VLAN](Screenshot_17.png){ #fig:017 width=80% }

Аналогичная проверка была выполнена с других рабочих станций. Например, с компьютера dk-pavlovskaya-dgavadadev-1 был выполнен ping к устройству из той же сети.

C:\>ping 10.128.3.5

Reply from 10.128.3.5: bytes=32 time<1ms TTL=128  
Reply from 10.128.3.5: bytes=32 time<1ms TTL=128  
Reply from 10.128.3.5: bytes=32 time<1ms TTL=128  
Reply from 10.128.3.5: bytes=32 time<1ms TTL=128  

Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)

При попытке обращения к устройству из другой сети ответы не поступают.

![Проверка связи с другого ПК](Screenshot_17.png){ #fig:018 width=80% }

Также была выполнена проверка сетевой связности между серверами. Например, с сервера web-dgavadadev был выполнен ping к серверам mail и file.

C:\>ping 10.128.0.4  
C:\>ping 10.128.0.3  

В обоих случаях были получены ответы от удалённых узлов, что подтверждает корректную работу сети серверного сегмента.

![Проверка связи между серверами](Screenshot_18.png){ #fig:019 width=80% }

## Анализ передачи ICMP-пакета в режиме Simulation

Для изучения процесса передачи пакетов был использован режим Simulation в Cisco Packet Tracer. В данном режиме можно наблюдать последовательность прохождения пакета через сетевые устройства.

Был отправлен ICMP-запрос (ping) от компьютера dk-pavlovskaya-dgavadadev-1 к компьютеру dk-donskaya-dgavadadev-1. В окне Simulation отображается последовательность прохождения пакета через коммутаторы сети.

![Движение пакета в режиме Simulation](Screenshot_19.png){ #fig:020 width=80% }

При анализе пакета была открыта информация PDU, где можно изучить структуру передаваемого кадра и заголовки используемых протоколов.

В составе Ethernet-кадра присутствуют следующие поля:

PREAMBLE — служебная последовательность для синхронизации передачи  
SFD — признак начала кадра  
DEST ADDR — MAC-адрес назначения  
SRC ADDR — MAC-адрес источника  
TYPE — тип протокола верхнего уровня  

Далее располагается IP-заголовок, содержащий основные параметры сетевого уровня:

VER — версия IP-протокола  
IHL — длина заголовка  
TTL — время жизни пакета  
SRC IP — IP-адрес источника  
DST IP — IP-адрес назначения  

После IP-заголовка следует заголовок протокола ICMP, содержащий поля:

TYPE — тип сообщения  
CODE — код сообщения  
CHECKSUM — контрольная сумма  
SEQ NUMBER — номер последовательности пакета  

![Структура ICMP пакета](Screenshot_20.png){ #fig:021 width=80% }

# Итоги

## Вывод

В результате выполнения работы была построена топология сети в Cisco Packet Tracer и выполнена настройка коммутаторов с использованием протокола VTP. На коммутаторе msk-donskaya-dgavadadev-sw-1 был настроен режим VTP-сервера и созданы необходимые VLAN, после чего остальные коммутаторы были настроены в режиме VTP-клиентов и автоматически получили конфигурацию VLAN.

Также были настроены магистральные соединения между коммутаторами с использованием Trunk-портов и выполнено распределение пользовательских портов по соответствующим VLAN. На серверах и оконечных устройствах были заданы статические IP-адреса и настроены шлюзы по умолчанию.

После завершения настройки была выполнена проверка сетевой связности с помощью команды ping. Проверка показала доступность устройств внутри одного VLAN и недоступность устройств, принадлежащих различным VLAN, что подтверждает корректную работу сегментации сети.

## Контрольные вопросы

**1. Какая команда используется для просмотра списка VLAN на сетевом устройстве?**  
Для просмотра списка VLAN на коммутаторе Cisco используется команда:

- `show vlan brief` — отображает список всех VLAN, настроенных на устройстве, их номера, имена и порты, принадлежащие каждому VLAN.

Также могут использоваться дополнительные команды:

- `show vlan` — отображает подробную информацию о VLAN.  
- `show interfaces trunk` — показывает интерфейсы, работающие в режиме trunk, и список VLAN, передаваемых через них.

**2. Охарактеризуйте VLAN Trunking Protocol (VTP). Приведите перечень команд с пояснениями для настройки и просмотра информации о VLAN.**  

VLAN Trunking Protocol (VTP) — это протокол Cisco, предназначенный для централизованного управления VLAN в сети коммутаторов. Он позволяет автоматически распространять информацию о созданных VLAN между коммутаторами, подключёнными через trunk-соединения.

Основные режимы работы VTP:

- **Server** — коммутатор может создавать, изменять и удалять VLAN. Информация распространяется на другие коммутаторы.  
- **Client** — коммутатор получает информацию о VLAN от VTP-сервера, но не может изменять её.  
- **Transparent** — коммутатор не участвует в VTP, но пересылает VTP-сообщения другим устройствам.

Основные команды настройки VTP:

- `vtp mode server` — переводит коммутатор в режим VTP-сервера.  
- `vtp mode client` — переводит коммутатор в режим клиента.  
- `vtp mode transparent` — переводит коммутатор в прозрачный режим.  
- `vtp domain <имя>` — задаёт имя VTP-домена.  
- `vtp password <пароль>` — устанавливает пароль для синхронизации VLAN.  

Команды для просмотра информации:

- `show vtp status` — отображает состояние VTP и параметры домена.  
- `show vlan brief` — показывает список VLAN, полученных через VTP.

**3. Охарактеризуйте Internet Control Message Protocol (ICMP). Опишите формат пакета ICMP.**  

Internet Control Message Protocol (ICMP) — это сетевой протокол, используемый для передачи служебных сообщений и диагностики работы сети. Он применяется для проверки доступности узлов и передачи сообщений об ошибках.

Наиболее известное применение ICMP — команда `ping`, которая отправляет ICMP Echo Request и получает ICMP Echo Reply.

Формат пакета ICMP включает следующие основные поля:

- **Type** — тип сообщения (например, Echo Request или Echo Reply).  
- **Code** — код сообщения, уточняющий тип ошибки или события.  
- **Checksum** — контрольная сумма для проверки целостности пакета.  
- **Identifier** — идентификатор сообщения.  
- **Sequence Number** — номер последовательности пакета.  
- **Data** — данные, передаваемые в пакете.

ICMP работает поверх протокола IP и используется для диагностики сетевых соединений.

**4. Охарактеризуйте Address Resolution Protocol (ARP). Опишите формат пакета ARP.**  

Address Resolution Protocol (ARP) — это сетевой протокол, предназначенный для определения MAC-адреса устройства по известному IP-адресу в локальной сети.

Когда устройство хочет отправить пакет другому узлу в сети, оно сначала определяет его MAC-адрес. Если MAC-адрес неизвестен, отправляется ARP-запрос, на который целевое устройство отвечает ARP-ответом.

Формат пакета ARP включает следующие поля:

- **Hardware Type** — тип аппаратной сети (например, Ethernet).  
- **Protocol Type** — тип сетевого протокола (обычно IPv4).  
- **Hardware Address Length** — длина MAC-адреса.  
- **Protocol Address Length** — длина IP-адреса.  
- **Operation** — тип операции (запрос или ответ).  
- **Sender MAC Address** — MAC-адрес отправителя.  
- **Sender IP Address** — IP-адрес отправителя.  
- **Target MAC Address** — MAC-адрес получателя.  
- **Target IP Address** — IP-адрес получателя.

**5. Что такое MAC-адрес? Какова его структура?**  

MAC-адрес (Media Access Control Address) — это уникальный физический адрес сетевого интерфейса, используемый на канальном уровне модели OSI для идентификации устройств в локальной сети.

MAC-адрес состоит из 48 бит (6 байт) и обычно записывается в шестнадцатеричном формате, например:

00:1A:2B:3C:4D:5E

Структура MAC-адреса включает две части:

- **OUI (Organizationally Unique Identifier)** — первые 24 бита, идентификатор производителя сетевого оборудования.  
- **NIC Specific** — последние 24 бита, уникальный номер устройства, присвоенный производителем.
