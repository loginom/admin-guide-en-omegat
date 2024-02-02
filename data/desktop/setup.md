# Loginom Desktop Installation

The installer file name versions:

* LoginomPersonal_7.x.x.msi — инсталлятор для редакции Personal
* LoginomCommunity_7.x.x.msi — инсталлятор для редакции Community

где 7.x.x – цифры, обозначающие версию и релиз программы.

> **Важно:** При переходе с Loginom 6.Х на 7.X нужно обновить [лицензионный ключ](../licenses_general/README.md).

## MSI Installation

### Graphic Interface

#### Run the Installer

Для установки с нестандартными параметрами в диалоге **"Тип установки"** нажимаем кнопку **"Выборочная"**:

* Компонент **Loginom Personal** обязателен для установки.
* **Guardant** — устанавливает [драйверы ключей Guardant](https://www.guardant.ru/support/users/drivers/) (если требуется) и [мастер активации SP-ключей](../windows/licenses/README.md).
* **Firebird 4** — устанавливает клиент Firebird 4 со встроенным сервером.
* **MariaDB Client** — устанавливает встроенный клиент MariaDB Connector/C.
* **Документация** — файлы документации по работе с платформой.
* **Демопримеры** — файлы пакетов с демонстрационными сценариями.

All components are installed by default.

### Command Line

```cmd
msiexec /i "LoginomPersonal_7.x.x.msi" ключи_msi
```

* `msi_options` — it is possible to find allowable values executing the following command in the command line: `msiexec /?`. The following commands can be especially useful:
   * `/l* "%TEMP%\loginom.msi.log"` — activation of installation logging.
   * `/qn` — "silent" installation without graphic interface mapping.

## Licenses

To start the **Loginom Desktop** it is required to configure the license keys (refer to  [Применение лицензий](./licenses/README.md)).

При использовании сервера сетевых ключей требуется создать файл [GnClient.ini](https://dev.guardant.ru/pages/viewpage.action?pageId=1277980) в каталоге `"C:\ProgramData\Loginom\Personal"`.
