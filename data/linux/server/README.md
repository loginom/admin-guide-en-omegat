---
description: Loginom Server под Linux - общие сведения, системные требования.
---

# The Loginom Server

It is an analytical server responsible for loading tasks, calculations, model training, visualisation, rights management, etc. The Loginom Server is a key platform element required for teamwork.

The [Loginom Studio](../../studio/README.md) provides for server management, configuration, visualisation and other operations. Other platform components, with the exception of the [Desktop](../../desktop/README.md) component, can interact with the Loginom Server.

The Loginom Server is designated for deployment in the internal network when the work of more than one user from different working places is anticipated.

Loginom Server на операционную систему семейства Linux может быть установлен в одном из двух режимов работы:

* [родной (Native) режим](./loginom-linux.md);
* в [режиме контейнеризации](./install-docker-podman.md) (Docker или Podman).

## System Requirements

| Component | Minimum | Recommended |
|:--------- |:-------------|:------------- |
| OS | Ядро Linux версии 5.3 и выше <sup>1</sup>, <sup>2</sup> | |
| CPU | 4 core <sup>3</sup> | 16 core <sup>3</sup> (Users x 1.6 core + 1 core) |
| RAM | 8 GB | 16 GB (Users x 2GB + 2GB) |
| Disk Space | 200 GB | from 500 GB (+ User Data) |
| Ping delay | <50мс | <15мс |

> **Note:** system requirements can vary according to data volume and interaction with third-party applications.

<sup>1</sup> Имеется возможность использовать ядро Linux версии от 4.11, если нет необходимости в подключении Python-a.

<sup>2</sup> Дистрибутив должен содержать пакеты: glibc 2.17 и старше, libstdc++6 версии 6.1 и старше, libicu 61 и старше.

<sup>3</sup> Поддерживается работа на x64 процессорах Intel Core, AMD FX и более новых, содержащих инструкции [SSE4.2](https://wikipedia.org/wiki/SSE4#SSE4.2) (POPCNT, LZCNT).
