# Loginom Integrator Configuration

Loginom Integrator настраивается с помощью xml файла [Integrator.dll.config](https://ru.wikipedia.org/wiki/Web.config). Расположение файла по умолчанию — `%ProgramFiles%\Loginom\Integrator`.

## Connection with the Loginom Server

Loginom Integrator can support connections with several Loginom Server instances simultaneously.

В элементе `configuration` присутствует элемент `settings`, в который можно добавить следующие атрибуты:

* **swaggerUI** — атрибут принимает значение true/false и включает/отключает генерацию страницы `openapi/index.html`, которая реализует доступ к опубликованным сервисам через Swagger UI.
* **urlPathPrefix** — необязательный атрибут, значение которого используется как префикс к адресу всех конечных точек REST и SOAP. Значение по умолчанию — пустая строка.
   Например, если Loginom Integrator развёрнут по адресу `http://localhost/lgi` и атрибут `urlPathPrefix` не указан, то help-страница доступна по адресу:

   * для REST: `http://localhost/lgi/rest/help`
   * для SOAP: `http://localhost/lgi/soap?wsdl`

   Если `urlPathPrefix="abc"`, то адрес меняется на:

   * для REST: `http://localhost/lgi/abc/rest/help`
   * для SOAP: `http://localhost/lgi/abc/soap?wsdl`

   Описание конкретного пакета находится по адресу:

   * для REST: `/lgi/rest/<PackageName>/help` или `/lgi/abc/rest/<PackageName>/help`
   * для SOAP: `/lgi/soap/<PackageName>` или `/lgi/abc/soap/<PackageName>`

> **Примечание:** Чтобы в Loginom Integrator 7 использовались те же адреса, что и в версии 6, нужно указать `urlPathPrefix="Service.svc"`.

Для каждого используемого экземпляра Loginom Server в элементе `configuration/settings` требуется добавить элемент `server` со следующими атрибутами:

* **unixsocket** — атрибут, позволяющий подключиться через Unix domain socket. Значение по умолчанию: /run/loginom/loginomd.socket
* **address** — сетевой адрес хоста Loginom Server. The required attribute. Default value: localhost.
* **port** — TCP [порт сервера](../server/setup.md#parametry-loginom-server). Значение по умолчанию: 4580.
* **userName** — имя учетной записи с правом на вход в качестве службы. If the attribute is not specified, it is required to use the service user login and password.
* **password** — пароль учетной записи с правом на вход в качестве службы. Default blank row.
* **reserved** — атрибут со значением "true" помечает сервер как резервный.
* **packageRefreshPeriod** - период (в секундах), с которым Integrator запрашивает с сервера список опубликованных пакетов. Integrator сравнивает этот список со своим собственным и если есть изменения, то применяет их. Если параметр отсутствует, то периодического запроса списка пакетов не происходит.

%spoiler% Пример config файла %spoiler%

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <configSections>
    <section name="log" type="Integrator.Core.LogConfigurationSection, IntegratorCore" />
    <section name="loginom" type="Integrator.Core.LoginomConfigurationSection, IntegratorCore" />
  </configSections>
  <log>
    <fileSystem level="Info" maxArchiveFiles="30" encoding="utf-8" />
    <eventLog level="Off" />
    <database connectionString="Provider=SQLNCLI11;Data Source=.\SQLEXPRESS;Integrated Security=SSPI;Initial Catalog=LgiLog" level="Off" table="Logs" dateColumn="Date" levelColumn="Level" machineNameColumn="MachineName" appDomainColumn="AppDomain" requestIdColumn="RequestId" packageNameColumn="PackageName" nodeNameColumn="NodeName" messageColumn="Message" exceptionColumn="Exception" requestColumn="Request" responseColumn="Response" />
    <!--sqlCommand="insert into Logs (Date, Level, MachineName, AppDomain, RequestId, PackageName, NodeName, Message, Exception, Request, Response)
              values (:date, :level, :machinename, :appdomain, :requestid, :packagename, :nodename, :message, :exception, :request, :response)"/>-->
    <console level="Off" />
    <journald level="Off" maxEntrySize="65536" />
    <internal level="Error" />
  </log>
  <settings swaggerUI="true" urlPathPrefix="Service.svc">
    <server unixsocket="/run/loginom/loginomd.socket" address="localhost" port="4580" userName="service" password="service" />
    <!--<server address="localhost" port="4580" userName="service" password="service" reserved="true"/>-->
  </settings>
</configuration>

```

%/spoiler%

Loginom Integrator maintains the information on operability of servers and packages published in them.

Upon receipt of request, the server is randomly selected among the available primary (not standby) servers providing the required package.

If there are no such servers, it is randomly selected among the available standby servers.

## Working Directory

The working directory contains the temporary data required for the application operation.

Путь к каталогу по умолчанию — `%ALLUSERSPROFILE%\Loginom\Integrator`. Переопределить значение можно в атрибуте `workDir` элемента `configuration/settings`. The value can be both a full and a relative (from the configuration file) path.

If several Loginom Integrator instances are used, the working directories must be different.

## Logging

The logging parameters must be specified in the `configuration/log` element.

It is possible to enable the following modes:

* **fileSystem** — запись в файл;
* **eventLog** — запись в журнал событий Windows;
* **database** — запись в базу данных;
* **internal** — запись в файл событий логирования (к примеру, ошибок при записи логов в базу данных).

It is possible to assign the minimum level of detail to each mode:

* **All** — все события;
* **Trace** — трассировка;
* **Debug** — отладка;
* **Info** — информация;
* **Warn** — предупреждения;
* **Error** — ошибки;
* **Fatal** — критические ошибки;
* **Off** — журналирование отключено.

При установке по умолчанию включена запись в файл с уровнем детализации Info, журналы пишутся в каталог `%ALLUSERSPROFILE%\Loginom\Integrator\Logs\`.

> **Note**: Request texts and service responses are written in the log providing the `Trace` logging level or higher.

### Write to File

Parameters of writing to file are specified in the `configuration/log/fileSystem` element:

* **path** — полный или относительный (от рабочего каталога) путь к каталогу журналов. Default value: `Logs`.
* **maxArchiveFiles** — максимальное количество архивных журналов. In the case of "0" value number of files is not limited. Default value: 0;
* **level** — минимальный уровень журналирования. Default value: `All`.

Имя файла журнала — `log.log`.

New files are created once a day, old log files receive names of the `log.yyyy-MM-dd.log` type.

### Write to Windows Events Log

Parameters of writing to Windows events log are specified in the `configuration/log/eventLog` element:

* **level** — минимальный уровень журналирования. Default value: `All`.

Event source in Windows log: `Loginom Integrator`.

### Write to Database

Parameters of writing to database are specified in the `configuration/log/database` element:

* **connectionString** — строка подключения к базе данных. The required attribute.
* **level** — минимальный уровень журналирования. Default value: `All`.

Target fields for the event data record can be specified in two ways: it is possible to bind the log fields to the table fields or to write SQL query. Для этого нужно задать атрибут `table` или `sqlCommand`, но не оба сразу.
> **Важно**: Если заданы два атрибута: `table` и `sqlCommand`, то Integrator выдаст ошибку при запуске.

#### Binding to the table fields

It is required to set the following attributes for binding:

* **table** — имя таблицы;
* **dateColumn** — имя поля для записи времени;
* **levelColumn** — имя поля для записи уровня события;
* **machineNameColumn** — имя поля для записи NetBIOS имени хоста, на котором запущен Integrator;
* **userNameColumn** — имя поля для записи пользователя, от имени которого запущен процесс Integrator;
* **appDomainColumn** — имя поля для записи идентификатора домена приложения;
* **requestIdColumn** — имя поля для записи уникального идентификатора запроса;
* **packageNameColumn** — имя поля для записи имени исполняемого пакета;
* **nodeNameColumn** — имя поля для записи имени исполняемого узла;
* **messageColumn** — имя поля для записи текста события;
* **exceptionColumn** — имя поля для записи текста ошибки;
* **requestColumn** — имя поля для записи текста запроса к веб-сервису;
* **responseColumn** — имя поля для записи текста ответа веб-сервиса.

Attributes with field names can be absent or contain blank values. In this regard, corresponding data must not be logged.

%spoiler%Пример скрипта создания таблицы хранения логов в MS SQL%spoiler%

```SQL
/* fields dimension correction can be required: nvarchar */
CREATE TABLE [dbo].[Logs](
    [Date] [datetime2](4),
    [Level] [nvarchar](10),
    [MachineName] [nvarchar](100),
    [AppDomain] [nvarchar](200),
    [RequestId] [nvarchar](32),
    [PackageName] [nvarchar](100) NULL,
    [NodeName] [nvarchar](100) NULL,
    [Message] [nvarchar](max) NULL,
    [Exception] [nvarchar](max) NULL,
    [Request] [nvarchar](max) NULL,
    [Response] [nvarchar](max) NULL
) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]
```

%/spoiler%

%spoiler%Binding of the log fields to the table fields. Пример раздела `<log>` Integrator.dll.config%spoiler%

```XML
<!--Параметры логирования задаются в элементе log/>-->
<log>
 <fileSystem level="Off" maxArchiveFiles="30" encoding="utf-8" path="C:\Logs"/>
 <eventLog level="Off" />
 <!--Элемент database содержит настройки логирования в БД/>-->
    <!--ConnectionString attribute contains the string of connection to the DB/>-->
    <!--OLEDB driver for SQLServer is used in the example (it must be installed in the system)/>-->
    <database connectionString="Provider=sqloledb; Server=192.168.0.1; Database=Integrator; User Id=sa; Password=Password123" level="All" table="Logs" dateColumn="Date" levelColumn="Level" machineNameColumn="MachineName" appDomainColumn="AppDomain" requestIdColumn="RequestId" packageNameColumn="PackageName" nodeNameColumn="NodeName" messageColumn="Message" exceptionColumn="Exception" requestColumn="Request" responseColumn="Response"/>
 <internal level="Error" />
</log>
```

%/spoiler%

#### SQL Query

An SQL query is specified in the `sqlCommand` attribute.

It is required to specify the following parameters in the query as set values:

* **:date** — время события;
* **:level** — уровень события;
* **:machinename** — NetBIOS имя хоста, на котором запущен Integrator;
* **:username** — имя пользователя, от имени которого запущен процесс Integrator;
* **:appdomain** — идентификатор домена приложения;
* **:requestid** — уникальный идентификатор запроса;
* **:packagename** — имя исполняемого пакета;
* **:nodename** — имя исполняемого узла;
* **:message** — текст события;
* **:exception** — текст ошибки;
* **:request** — текст запроса к веб-сервису;
* **:response** — текст ответа веб-сервиса.

> **Note**: Use of `userNameColumn` attribute and its corresponding `:username` parameter significantly increases the logging operation execution time. It is recommended not to use them unless necessary.

%spoiler%Write the log fields by the SQL query. Пример раздела `<log>` Integrator.dll.config%spoiler%

```XML
<!--Параметры логирования задаются в элементе log/>-->
<log>
 <fileSystem level="Off" maxArchiveFiles="30" encoding="utf-8" path="C:\Logs"/>
 <eventLog level="Off" />
 <!--Элемент database содержит настройки логирования в БД/>-->
    <!--ConnectionString attribute contains the string of connection to the DB/>-->
    <!--OLEDB driver for SQLServer is used in the example (it must be installed in the system)/>-->
    <!--sqlCommand attribute sets the request to write the log events/>-->
    <!--Such logging method enables to modify the log records/>-->
    <!--The contents of :response field in the example will be cut up to 1000 signs/>-->
    <database connectionString="Provider=sqloledb; Server=192.168.0.1; Database=Integrator; User Id=sa; Password=Password123" level="All" sqlCommand="insert into Logs (Date, Level, MachineName, AppDomain, RequestId, PackageName, NodeName, Message, Exception, Request, Response) values (:date, :level, :machinename, :appdomain, :requestid, :packagename, :nodename, :message, :exception, :request, SUBSTRING( :response, 0, 999))"/>
    <internal level="Error" />
</log>
```

%/spoiler%

### Logging events record

The parameters of logging events record are specified in the `configuration/log/internal` element:

* **path** — полный или относительный (от рабочего каталога) путь к каталогу журналов. Default value: `Logs`.
* **level** — минимальный уровень журналирования. Default value: `All`.

Имя файла журнала — `logger-internal.log`.
