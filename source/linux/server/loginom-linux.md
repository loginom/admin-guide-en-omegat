---
description: Установка Loginom на Linux без систем контейнеризации (в качестве службы).
---

# Установка Loginom Сервер  в качестве службы

> **Важно**:
> * минимальная версия ядра `Linux` – `5.9`.
> * в процессе установки необходимо наличие доступа в интернет!
> * для работы службы `loginomd` необходим установленный [Лицензионный ключ](../licenses/sp-key-activate.md)

## Процесс установки

Дистрибутив `Loginom` содержит три папки с основными [компонентами](https://help.loginom.ru/userguide/components-platform.html):

* `/server` содержит [Loginom Server](./README.md);
* `/http` содержит веб-сервер Apache;
* `/integrator` содержит [Loginom Integrator](../integrator/README.md).

Установка каждого компонента производится с помощью скрипта, который находится в папке компонента.

Для установки нужно сделать следующее:

1. Перенести дистрибутив на целевую машину любым удобным способом.

2. Разархивировать дистрибутив.

    ```cmd
    mkdir loginom-dist && tar -xf loginom.tar.gz -C loginom-dist/
    ```

3. Перейти в папку `/loginom-dist/server/` и выполнить установку `Loginom-server`.

    ```cmd
    sudo ./install_server.sh
    ```

4. Перейти в папку `/loginom-dist/http/` и выполнить установку `Loginom-http`.

    ```cmd
    sudo ./install_http.sh
    ```

5. Перейти в папку `/loginom-dist/integrator/` и выполнить установку `Loginom-integrator`.

    ```cmd
    sudo ./install_integrator.sh
    ```

6. Перед запуском службы `loginomd` в строке IP_NAME= в файле, расположенном по умолчанию `/var/opt/loginom/server/gnclient.ini`, указать ip-адрес машины.

7. Запустить службы `Loginom-server` и `Loginom-integrator`.

    ```cmd
    sudo systemctl start loginomd
    sudo systemctl start loginomi
    ```

## Каталоги Loginom

### Loginom-http

Веб-сервером является `apache2`.

Каталоги стандартные для `apache2`:

*  `/etc/apache2` – конфигурационные файлы,
*  `/var/www/html/client` – директория с файлами клиента.

### Loginom-server

Каталог с бинарными файлами: `/opt/loginom/server`.

Владельцем файлов является пользователь **root**, права доступа к файлам `664` (`775` для исполняемых).

Рабочая директория: `/var/opt/loginom/server`, владельцем файлов является пользователь **loginom**, права доступа к файлам `660`.
Сервер работает в качестве службы `loginom-server` с алиасом `loginomd`.

Параметры службы:

* Служба работает от пользователя **loginom**, создаваемые службой файлы имеют права `660`;
* `OOMScoreAdjust=-200` (уменьшаем баллы для процесса, задающие приоритет завершения сервиса вследствие срабатывания механизма `OOM`);
* `TimeoutStopSec=11` (таймаут остановки, сек.);
* `Restart=on-failure RestartSec=30` (рестарт службы при сбое через 30 секунд).

### Loginom-integrator

Каталог с бинарными файлами: `/opt/loginom/integrator`.

Владельцем файлов является пользователь **root**, права доступа к файлам `664` (`775` для исполняемых).

Рабочая директория: `/var/opt/loginom/integrator`.

Владельцем файлов является пользователь **loginom**, права доступа к файлам `660`.

`Loginom-integrator` работает в качестве службы `loginom-integrator` с алиасом `loginomi`.

Параметры службы:

* Служба работает от пользователя **loginom**, создаваемые службой файлы имеют права `660`;
* `OOMScoreAdjust=-100` (уменьшаем баллы для процесса, задающие приоритет завершения сервиса вследствие срабатывания механизма `OOM`);
* `TimeoutStopSec=6` (таймаут остановки, сек.);
* `Restart=on-failure RestartSec=30` (рестарт службы при сбое через 30 секунд).
