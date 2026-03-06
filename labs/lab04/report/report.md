---
## Front matter
title: "Отчёт по лабораторной работе 4"
subtitle: "Первоначальное конфигурирование сети"
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

Провести подготовительную работу по первоначальной настройке коммутаторов сети.

# Процесс работы

## Построение топологии сети в Packet Tracer

В логической рабочей области Cisco Packet Tracer была построена топология сети согласно схеме L1.  
На рабочем поле были размещены коммутаторы серии 2950/2960, персональные компьютеры и серверы.

Каждому устройству было присвоено имя в соответствии со структурой сети. После размещения устройства были соединены между собой через соответствующие интерфейсы Ethernet.

В результате была сформирована топология локальной сети, включающая несколько коммутаторов доступа, пользовательские рабочие станции и серверы.

![Топология сети в Packet Tracer](Screenshot_1.png){ #fig:001 width=80% }

## Первоначальная настройка коммутаторов

Для каждого коммутатора была выполнена базовая конфигурация. Настройка включала изменение имени устройства, настройку IP-адреса управления, указание шлюза по умолчанию, настройку паролей доступа, включение шифрования паролей, создание пользователя администратора, генерацию RSA-ключей и включение удалённого доступа по SSH.

## Настройка коммутатора msk-donskaya-dgavadadev-sw-1

Для первого коммутатора была выполнена первоначальная настройка. В режиме конфигурации было изменено имя устройства, настроен интерфейс управления VLAN2 и задан IP-адрес управления.

enable  
configure terminal  
hostname msk-donskaya-dgavadadev-sw-1  
interface vlan2  
no shutdown  
ip address 10.128.1.2 255.255.255.0  
exit  
ip default-gateway 10.128.1.1  

Также были настроены параметры удалённого доступа, включено шифрование паролей и создан пользователь администратора.

![Настройка коммутатора msk-donskaya-dgavadadev-sw-1](Screenshot_2.png){ #fig:002 width=80% }

## Настройка коммутатора msk-donskaya-dgavadadev-sw-2

Для второго коммутатора была выполнена аналогичная базовая настройка. Устройству было присвоено имя msk-donskaya-dgavadadev-sw-2, после чего был настроен интерфейс управления VLAN2.

hostname msk-donskaya-dgavadadev-sw-2  
interface vlan2  
no shutdown  
ip address 10.128.1.3 255.255.255.0  
ip default-gateway 10.128.1.1  

После этого были настроены параметры доступа через консоль и виртуальные терминалы, включено шифрование паролей и настроен доступ по SSH.

![Настройка коммутатора msk-donskaya-dgavadadev-sw-2](Screenshot_3.png){ #fig:003 width=80% }

## Настройка коммутатора msk-donskaya-dgavadadev-sw-3

На третьем коммутаторе была выполнена аналогичная базовая настройка. Имя устройства было изменено на msk-donskaya-dgavadadev-sw-3, после чего был настроен интерфейс VLAN2.

hostname msk-donskaya-dgavadadev-sw-3  
interface vlan2  
no shutdown  
ip address 10.128.1.4 255.255.255.0  
ip default-gateway 10.128.1.1  

Также были настроены линии console и vty, включено шифрование паролей, создан пользователь администратора и настроен доступ по протоколу SSH.

![Настройка коммутатора msk-donskaya-dgavadadev-sw-3](Screenshot_4.png){ #fig:004 width=80% }

## Настройка коммутатора msk-donskaya-dgavadadev-sw-4

На коммутаторе msk-donskaya-dgavadadev-sw-4 была выполнена настройка интерфейса управления VLAN2.

hostname msk-donskaya-dgavadadev-sw-4  
interface vlan2  
no shutdown  
ip address 10.128.1.5 255.255.255.0  
ip default-gateway 10.128.1.1  

После настройки интерфейса управления были настроены параметры удалённого доступа, создан пользователь администратора и включён протокол SSH.

![Настройка коммутатора msk-donskaya-dgavadadev-sw-4](Screenshot_5.png){ #fig:005 width=80% }

## Настройка коммутатора msk-pavlovskaya-dgavadadev-sw-1

На последнем коммутаторе была выполнена базовая конфигурация устройства. Коммутатору было назначено имя msk-pavlovskaya-dgavadadev-sw-1, после чего был настроен интерфейс VLAN2 для удалённого управления.

hostname msk-pavlovskaya-dgavadadev-sw-1  
interface vlan2  
no shutdown  
ip address 10.128.1.6 255.255.255.0  
ip default-gateway 10.128.1.1  

Далее были настроены линии удалённого доступа, включено шифрование паролей, создан пользователь администратора и выполнена генерация RSA-ключей для работы протокола SSH.

![Настройка коммутатора msk-pavlovskaya-dgavadadev-sw-1](Screenshot_6.png){ #fig:006 width=80% }

# Итоги

## Вывод  

В результате выполнения работы была построена топология сети в Cisco Packet Tracer и выполнена первоначальная настройка всех коммутаторов. Каждому устройству были назначены уникальные имена и IP-адреса управления, настроены параметры доступа, включено шифрование паролей и реализован защищённый удалённый доступ по протоколу SSH.

## Контрольные вопросы

**1. При помощи каких команд можно посмотреть конфигурацию сетевого оборудования?**  
Для просмотра текущей конфигурации сетевого оборудования используется команда:

- `show running-config` — отображает текущую конфигурацию устройства, которая хранится в оперативной памяти и используется в данный момент.

**2. При помощи каких команд можно посмотреть стартовый конфигурационный файл оборудования?**  
Для просмотра стартовой конфигурации используется команда:

- `show startup-config` — отображает конфигурацию, которая хранится в энергонезависимой памяти устройства и загружается при запуске оборудования.

**3. При помощи каких команд можно экспортировать конфигурационный файл оборудования?**  
Экспорт конфигурационного файла на внешний сервер можно выполнить при помощи команд копирования:

- `copy running-config tftp` — экспорт текущей конфигурации на TFTP-сервер.  
- `copy startup-config tftp` — экспорт стартовой конфигурации на TFTP-сервер.

**4. При помощи каких команд можно импортировать конфигурационный файл оборудования?**  
Импорт конфигурационного файла с внешнего сервера выполняется при помощи следующих команд:

- `copy tftp running-config` — загрузка конфигурации с TFTP-сервера в текущую конфигурацию устройства.  
- `copy tftp startup-config` — загрузка конфигурации с TFTP-сервера в стартовую конфигурацию устройства.