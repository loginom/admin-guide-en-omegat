---
description: Руководство администратора Loginom. Обновление программы.
---
# Обновление Loginom с 6.x.x на 7.x.x

> **Важно**: При обновлении с Loginom 6.x на Loginom 7.x требуются новые лицензионные ключи. Для SP-ключей - это активация нового файла лицензии, для USB-ключей - это обновление прошивки. Новые лицензионные ключи необходимо получить до начала запуска процедуры обновления.

Loginom 7 по умолчанию будет установлен по новому пути - `C:\Program Files\Loginom` и `C:\ProgramData\Loginom` (ранее он устанавливался по пути `C:\Program Files\BaseGroup\Loginom 6` и `C:\ProgramData\BaseGroup\Loginom 6`). Рекомендуем при установке Loginom 7 использовать пути по умолчанию. 

## Обновление серверных редакций 
Для обновления редакций Team, Standard, Enterprise необходимо:

1. Сделать резервную копию папок `Client`, `Server` и `Integrator` (расположены по пути `C:\Program Files\BaseGroup\Loginom 6`).
2. Установить [Loginom Server](./../windows/server/setup.md) и [Loginom Integrator](./../windows/integrator/setup.md) (при наличии) по инструкциям. Для Loginom Integrator 7 необходим компонент [ASP.NET Core Runtime 6.0](https://dotnet.microsoft.com/en-us/download/dotnet/6.0).

    2.1 После установки Loginom Integrator 7 изменить физический путь к виртуальному каталогу (по умолчанию `Диспетчер служб IIS: управление виртуальным каталогом → Дополнительные параметры...` → физический путь — `C:\Program Files\Loginom\Client`).

    2.2 В файле Integrator.dll.config (ранее *web.config*) в элементе loginom указать `urlPathPrefix="Service.svc"`.
3. Перенести папку *UserStorage* из `C:\ProgramData\BaseGroup\Loginom 6\Server` в `C:\ProgramData\Loginom\Server`.
4. Перенести файлы _*.cfg_, **кроме файла Settings.cfg**, из `C:\ProgramData\BaseGroup\Loginom 6\Server` в `C:\ProgramData\Loginom\Server` .
 
 После завершения процедуры обновления на рабочих местах конечных пользователей необходимо сбросить кэш браузера.

## Обновление настольных редакций
Для обновления  редакций Community и Personal необходимо
произвести установку согласно [инструкции](./../desktop/setup.md). После окончания установки новый ярлык на рабочем столе автоматически заменяет старый.
