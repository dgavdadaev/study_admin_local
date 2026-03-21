---
lang: ru-RU
title: Администрирование локальных сетей
subtitle: Лабораторная работа №6  
author:
  - Авдадаев Джамал Геланиевич
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 17 марта 2026

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

Настроить межвлановую маршрутизацию с использованием технологии Router-on-a-Stick и изучить процесс передачи пакетов в сети.

# Ход выполнения работы

## Построение топологии сети

![Топология сети](Screenshot_1.png){ width=70% }

## Первоначальная настройка маршрутизатора

![Настройка маршрутизатора](Screenshot_2.png){ width=70% }

## Настройка подинтерфейсов

![Настройка VLAN-интерфейсов](Screenshot_3.png){ width=70% }

## Конфигурация всех VLAN

![Полная конфигурация](Screenshot_4.png){ width=70% }

## Проверка связности

![Проверка сети](Screenshot_5.png){ width=70% }

## Анализ работы сети

![Simulation](Screenshot_7.png){ width=70% }

## Анализ структуры пакета

![Структура пакета](Screenshot_9.png){ width=70% }

# Выводы

## Итог лабораторной работы

В ходе выполнения лабораторной работы:

- реализована межвлановая маршрутизация  
- настроена технология Router-on-a-Stick  
- обеспечена связность между VLAN  
- изучен процесс передачи пакетов  
- проанализированы заголовки сетевых протоколов  
