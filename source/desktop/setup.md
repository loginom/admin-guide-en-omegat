# Установка Loginom Desktop

Варианты имени файла инсталлятора:

* LoginomPersonal_7.x.x.msi — инсталлятор для редакции Personal
* LoginomCommunity_7.x.x.msi — инсталлятор для редакции Community

где 7.x.x – цифры, обозначающие версию и релиз программы.

> **Важно:** При переходе с Loginom 6.Х на 7.X нужно обновить [лицензионный ключ](../licenses_general/README.md).

## Установка MSI

### Графический интерфейс

#### Запуск инсталлятора

Для установки с нестандартными параметрами в диалоге **"Тип установки"** нажимаем кнопку **"Выборочная"**:

* Компонент **Loginom Personal** обязателен для установки.
* **Guardant** — устанавливает [драйверы ключей Guardant](https://www.guardant.ru/support/users/drivers/) (если требуется) и [мастер активации SP-ключей](../windows/licenses/README.md).
* **Firebird 4** — устанавливает клиент Firebird 4 со встроенным сервером.
* **MariaDB Client** — устанавливает встроенный клиент MariaDB Connector/C.
* **Документация** — файлы документации по работе с платформой.
* **Демопримеры** — файлы пакетов с демонстрационными сценариями.

По умолчанию устанавливаются все компоненты.

### Командная строка

```cmd
msiexec /i "LoginomPersonal_7.x.x.msi" ключи_msi
```

* `ключи_msi` — допустимые значения можно узнать, выполнив в командной строке `msiexec /?`. Особо полезными могут быть:
  * `/l* "%TEMP%\loginom.msi.log"` — включение журналирования установки.
  * `/qn` — "тихая" установка без отображения графического интерфейса.

## Лицензии

Для запуска **Loginom Desktop** требуется настройка ключей лицензирования (см. [Применение лицензий](./licenses/README.md)).

При использовании сервера сетевых ключей требуется создать файл [GnClient.ini](https://dev.guardant.ru/pages/viewpage.action?pageId=1277980) в каталоге `"C:\ProgramData\Loginom\Personal"`.