---
lang: ru-RU
title: Администрирование локальных сетей
subtitle: Лабораторная работа №8  
author:
  - Авдадаев Джамал Геланиевич
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 30 марта 2026

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

Приобретение практических навыков по настройке сетевых сервисов DNS и DHCP в локальной сети, а также проверка автоматического получения IP-адресов клиентскими устройствами.

# Ход выполнения работы

## Добавление и подключение DNS-сервера

![Топология сети с DNS-сервером](Screenshot_1.png){ width=72% }

## Настройка DNS-сервера

![Настройка IP DNS-сервера](Screenshot_2.png){ width=48% }

## Настройка DNS-сервера

![Настройка службы DNS](Screenshot_3.png){ width=48% }

## Настройка DHCP на маршрутизаторе

![Настройка DHCP на маршрутизаторе](Screenshot_4.png){ width=72% }

## Проверка работы DHCP

![Информация о пулах DHCP](Screenshot_7.png){ width=48% }

## Проверка работы DHCP

![Таблица выданных адресов](Screenshot_8.png){ width=48% }

## Настройка клиентских устройств

![Получение IP-адреса по DHCP](Screenshot_5.png){ width=72% }

## Проверка связности сети

![Проверка связности сети](Screenshot_6.png){ width=72% }

## DHCP Discover

![DHCP Discover](Screenshot_9.png){ width=48% }

## DHCP Offer

![DHCP Offer](Screenshot_10.png){ width=48% }

## DHCP Request

![DHCP Request](Screenshot_11.png){ width=48% }

## DHCP ACK

![DHCP ACK](Screenshot_12.png){ width=48% }

# Выводы

## Итог лабораторной работы

В ходе выполнения лабораторной работы:

- добавлен и настроен DNS-сервер
- настроена служба DHCP на маршрутизаторе
- созданы пулы адресов для нескольких подсетей
- клиентские устройства переведены на динамическое получение адресов
- подтверждена корректная работа DHCP и сетевой связности
- изучен процесс обмена DHCP-сообщениями в режиме симуляции