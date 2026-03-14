---
lang: ru-RU
title: Администрирование локальных сетей
subtitle: Лабораторная работа №5  
author:
  - Авдадаев Джамал Геланиевич
institute:
  - Российский университет дружбы народов, Москва, Россия

babel-lang: russian
babel-otherlangs: english

toc: false
slide_level: 2
aspectratio: 169
section-titles: true
theme: metropolis

header-includes:
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
---

# Цели и задачи работы

## Цель лабораторной работы

Получить основные навыки по настройке VLAN на коммутаторах сети, изучить конфигурирование Trunk-портов, VTP, статической адресации и проверить сетевую связность в Cisco Packet Tracer.

# Ход выполнения работы

## Построение топологии сети

![Топология сети в Packet Tracer](Screenshot_1.png){ width=70% }

## Настройка VTP-сервера и создание VLAN

На коммутаторе msk-donskaya-dgavadadev-sw-1 был настроен режим VTP Server.  
Также были созданы VLAN:

- VLAN 2 — management
- VLAN 3 — servers
- VLAN 101 — dk
- VLAN 102 — departments
- VLAN 103 — adm
- VLAN 104 — other

## Настройка VTP-сервера и создание VLAN

![Настройка VTP-сервера и VLAN](Screenshot_2.png){ width=70% }

## Настройка коммутатора msk-donskaya-dgavadadev-sw-2

![Настройка коммутатора sw-2](Screenshot_3.png){ width=70% }

## Настройка коммутатора msk-donskaya-dgavadadev-sw-3

![Настройка коммутатора sw-3](Screenshot_4.png){ width=70% }

## Настройка коммутатора msk-donskaya-dgavadadev-sw-4

![Настройка коммутатора sw-4](Screenshot_5.png){ width=48% }\hfill

## Настройка коммутатора msk-pavlovskaya-dgavadadev-sw-1

![Настройка коммутатора pavlovskaya-sw-1](Screenshot_6.png){ width=48% }

## Настройка IP-адресов серверов

На серверах были заданы статические IP-адреса из сети 10.128.0.0/24:

- web-dgavadadev — 10.128.0.2
- file-dgavadadev — 10.128.0.3
- mail-dgavadadev — 10.128.0.4
- шлюз по умолчанию — 10.128.0.1

## Настройка IP-адресов оконечных устройств

Рабочим станциям были назначены статические IP-адреса из диапазонов соответствующих VLAN с указанием шлюза по умолчанию.

Примеры настроенных адресов:
- dk-donskaya-dgavadadev-1 — 10.128.3.5
- dk-pavlovskaya-dgavadadev-1 — 10.128.3.15
- dep-donskaya-dgavadadev-1 — 10.128.4.5
- adm-donskaya-dgavadadev-1 — 10.128.5.5
- other-donskaya-dgavadadev-1 — 10.128.6.5
- othet-pavlovskaya-dgavadadev-1 — 10.128.6.15

## Проверка связности с помощью ping

![Проверка связи внутри VLAN](Screenshot_16.png){ width=48% }\hfill

## Проверка связности с помощью ping

![Проверка связи между серверами](Screenshot_18.png){ width=48% }

## Анализ ICMP-пакета в режиме Simulation

![Движение пакета в режиме Simulation](Screenshot_19.png){ width=48% }\hfill

## Анализ ICMP-пакета в режиме Simulation

![Структура ICMP пакета](Screenshot_20.png){ width=48% }

# Выводы

## Итог лабораторной работы

В ходе выполнения лабораторной работы была построена топология сети в Cisco Packet Tracer и выполнена настройка коммутаторов с использованием протокола VTP. На коммутаторе msk-donskaya-dgavadadev-sw-1 был настроен режим VTP-сервера и созданы необходимые VLAN, после чего остальные коммутаторы были настроены в режиме VTP-клиентов и автоматически получили конфигурацию VLAN.

Также были настроены магистральные соединения между коммутаторами с использованием Trunk-портов и выполнено распределение пользовательских портов по соответствующим VLAN. На серверах и оконечных устройствах были заданы статические IP-адреса и настроены шлюзы по умолчанию.

После завершения настройки была выполнена проверка сетевой связности с помощью команды ping. Проверка показала доступность устройств внутри одного VLAN и недоступность устройств, принадлежащих различным VLAN, что подтверждает корректную работу сегментации сети.