---
lang: ru-RU
title: Использование протокола STP
subtitle: Лабораторная работа №9. Агрегирование каналов
author:
  - Авдадаев Джамал Геланиевич
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 05 марта 2026

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

Изучить возможности протокола STP и его модификаций по обеспечению отказоустойчивости сети, а также освоить принципы агрегирования каналов EtherChannel.

# Ход выполнения работы

## Изменённая топология сети

![Изменённая топология сети](Screenshot_1.png){ width=78% }

## Движение пакетов в режиме симуляции

![Проверка прохождения пакетов](Screenshot_2.png){ width=72% }

## Состояние STP на коммутаторе sw-2

![Состояние STP](Screenshot_4.png){ width=70% }

## Назначение root bridge

![Настройка корневого коммутатора](Screenshot_5.png){ width=70% }

## Настройка PortFast

![Настройка PortFast](Screenshot_8.png){ width=70% }

## Потери пакетов при работе STP

![Отказоустойчивость STP](Screenshot_10.png){ width=82% }

## Включение Rapid PVST+

![Настройка Rapid PVST+](Screenshot_11.png){ width=70% }

## Проверка работы Rapid PVST+

![Работа Rapid PVST+](Screenshot_12.png){ width=82% }

## Настройка EtherChannel

## формирование EtherChannel

![формирование EtherChannel](Screenshot_13.png){ width=70% }

## Сформированное агрегированное соединение

![Агрегированный канал между коммутаторами](Screenshot_14.png){ width=78% }

# Выводы

## Итоги лабораторной работы

В результате выполнения лабораторной работы:

- исследована работа протокола STP и его влияние на отказоустойчивость сети
- выполнено переключение сети на Rapid PVST+
- подтверждено уменьшение времени восстановления соединения
- настроен режим PortFast на серверных портах
- реализовано агрегирование каналов EtherChannel
- подтверждена работоспособность сети после объединения физических интерфейсов в port-channel

