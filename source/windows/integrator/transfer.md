# Перенос Loginom Integrator на другой компьютер

Для переноса Loginom Integrator необходимо поменять его настройки в web-config. По умолчанию он находится в `C:\ProgramData\Loginom\Integrator`.

Loginom Integrator может поддерживать соединения с несколькими экземплярами Loginom Server одновременно.

Для каждого используемого экземпляра Loginom Server в элементе `configuration/loginom` требуется добавить элемент server со следующими атрибутами:

* **address** — сетевой адрес хоста Loginom Server. Обязательный атрибут. Значение по умолчанию: **localhost**.

* **port** — TCP [порт сервера](https://help.loginom.ru/adminguide/windows/server/setup.html#parametry-loginom-server). Значение по умолчанию: **4850**.

* **userName** — имя учетной записи с правом на вход в качестве службы. Если атрибут не задан, используются логин и пароль пользователя **service**.

* **password** — пароль учетной записи с правом на вход в качестве службы. По умолчанию пустая строка.

* **reserved** — атрибут со значением "true" помечает сервер как резервный.

Подробнее можно прочитать в [Конфигурации Loginom Integrator](./data/../../integrator/config.md).

> **Важно:** версии Loginom и Integrator должны совпадать.
