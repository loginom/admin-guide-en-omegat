# Loginom Desktop для Linux

The desktop version of the platform is designated for installation on the local computer when only one user operation is anticipated.

Loginom Desktop способна обработать большие массивы данных, ограниченные только ресурсами рабочей станции. Loginom Desktop не включает инструменты для совместной работы, разграничения прав и публикации веб-сервисов.

The Desktop component is included in the Loginom Personal and Loginom Community platform editions.

## System Requirements

| Component | Minimum | Recommended |
|:--------- |:-------------|:------------- |
| OS | Дистрибутивы, поддерживающие GTK3, с минимальной версией ядра 5.3 <sup>1</sup>, <sup>2</sup> | |
| CPU | 2 core <sup>3</sup> | 16 core <sup>3</sup> |
| RAM | 4 GB | 8 GB |
| Disk Space | 1 GB | 10 GB (+ User Data) |

> **Note:** system requirements can vary according to data volume and interaction with third-party applications.

<sup>1</sup> Имеется возможность использовать ядро Linux версии от 4.11, если нет необходимости в подключении Python-a.

<sup>2</sup> В некоторых случаях используются утилиты `xdg-open` и `dbus-send`, значение переменной окружения `DESKTOP_SESSION`.

<sup>3</sup> Поддерживается работа на x64 процессорах Intel Core, AMD FX и более новых, содержащих инструкции [SSE4.2](https://wikipedia.org/wiki/SSE4#SSE4.2) (POPCNT, LZCNT).
