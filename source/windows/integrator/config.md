# Конфигурация Loginom Integrator

Loginom Integrator настраивается с помощью xml файла [Integrator.dll.config](https://ru.wikipedia.org/wiki/Web.config). Расположение файла по умолчанию — `%ProgramFiles%\Loginom\Integrator`.

## Соединение с Loginom Server

Loginom Integrator может поддерживать соединения с несколькими экземплярами Loginom Server одновременно.

Для каждого используемого экземпляра Loginom Server в элементе `configuration/loginom` требуется добавить элемент `server` со следующими атрибутами:

* **address** — сетевой адрес хоста Loginom Server. Обязательный атрибут. Значение по умолчанию: localhost.
* **port** — TCP [порт сервера](../server/setup.md#parametry-loginom-server). Значение по умолчанию: 4580.
* **userName** — имя учетной записи с правом на вход в качестве службы. Если атрибут не задан, используются логин и пароль пользователя service.
* **password** — пароль учетной записи с правом на вход в качестве службы. По умолчанию пустая строка.
* **reserved** — атрибут со значением "true" помечает сервер как резервный.
* **swaggerUI** — атрибут принимает значение true/false и включает/отключает генерацию страницы `openapi/index.html`, которая реализует доступ к опубликованным сервисам через Swagger UI.
* **urlPathPrefix** — необязательный атрибут, значение которого используется как префикс к адресу всех конечных точек REST и SOAP. Значение по умолчанию — пустая строка.
Например, если Loginom Integrator развёрнут по адресу `http://localhost/lgi` и атрибут `urlPathPrefix` не указан, то help-страница для REST сервисов доступна по адресу `http://localhost/lgi/rest/help`. Если `urlPathPrefix="abc"`, то адрес меняется на `http://localhost/lgi/abc/rest/help`.

> **Примечание:** Чтобы в Loginom Integrator 7 использовались те же адреса, что и в версии 6, нужно указать `urlPathPrefix="Service.svc"`.

%spoiler% Пример config файла %spoiler%

``` xml
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
  <loginom swaggerUI="true" urlPathPrefix="Service.svc">
    <server unixsocket="/run/loginom/loginomd.socket" address="localhost" port="4580" userName="service" password="service" />
    <!--<server address="localhost" port="4580" userName="service" password="service" reserved="true"/>-->
  </loginom>
</configuration>

```

%/spoiler%

Loginom Integrator поддерживает информацию о работоспособности серверов и опубликованных на них пакетах.

При поступлении запроса сервер выбирается случайным образом среди доступных основных (не резервных) серверов, предоставляющих требуемый пакет.

Если таких нет, то сервер выбирается случайным образом среди доступных резервных серверов.

## Рабочий каталог

Рабочий каталог содержит временные данные, требующиеся для работы приложения.

Путь к каталогу по умолчанию — `%ALLUSERSPROFILE%\Loginom\Integrator`. Переопределить значение можно в атрибуте `workDir` элемента `configuration/loginom`. Значением может быть полный или относительный (от файла конфигурации) путь.

Если используется несколько экземпляров Loginom Integrator, рабочие каталоги у них должны быть разными.

## Журналирование

Параметры журналирования задаются в элементе `configuration/log`.

Возможно включение следующих режимов:

* **fileSystem** — запись в файл;
* **eventLog** — запись в журнал событий Windows;
* **database** — запись в базу данных;
* **internal** — запись в файл событий логирования (к примеру, ошибок при записи логов в базу данных).

Каждому режиму можно задать минимальный уровень детализации:

* **All** — все события;
* **Trace** — трассировка;
* **Debug** — отладка;
* **Info** — информация;
* **Warn** — предупреждения;
* **Error** — ошибки;
* **Fatal** — критические ошибки;
* **Off** — журналирование отключено.

При установке по умолчанию включена запись в файл с уровнем детализации Info, журналы пишутся в каталог `%ALLUSERSPROFILE%\Loginom\Integrator\Logs\`.

> **Примечание**: запись в лог текстов запросов и ответов сервиса осуществляется при уровне логирования не ниже `Trace`.

### Запись в файл

Параметры записи в файл задаются в элементе `configuration/log/fileSystem`:

* **path** — полный или относительный (от рабочего каталога) путь к каталогу журналов. Значение по умолчанию: `Logs`.
* **maxArchiveFiles** — максимальное количество архивных журналов. При значении "0" количество файлов не ограничено. Значение по умолчанию: 0;
* **level** — минимальный уровень журналирования. Значение по умолчанию: `All`.

Имя файла журнала — `log.log`.

Новые файлы создаются раз в сутки, старые лог-файлы получают имена вида `log.yyyy-MM-dd.log`.

### Запись в журнал событий Windows

Параметры записи в журнал событий Windows задаются в элементе `configuration/log/eventLog`:

* **level** — минимальный уровень журналирования. Значение по умолчанию: `All`.

Источник событий в журнале Windows: `Loginom Integrator`.

### Запись в базу данных

Параметры записи в базу данных задаются в элементе `configuration/log/database`:

* **connectionString** — строка подключения к базе данных. Обязательный атрибут.
* **level** — минимальный уровень журналирования. Значение по умолчанию: `All`.

Целевые поля для записи данных события можно задать двумя способами: привязать поля журнала к полям таблицы, либо задать SQL запрос.

#### Привязка к полям таблицы

Для привязки требуется задать следующие атрибуты:

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

Атрибуты с именами полей могут отсутствовать или содержать пустые значения. В этом случае соответствующие данные не логируются.

%spoiler%Пример скрипта создания таблицы хранения логов в MS SQL%spoiler%

``` SQL
/* может потребоваться корректировка размерности полей nvarchar */
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

%spoiler%Привязка полей журнала к полям таблицы. Пример раздела `<log>` Integrator.dll.config%spoiler%

``` XML
<!--Параметры логирования задаются в элементе log/>-->
<log>
 <fileSystem level="Off" maxArchiveFiles="30" encoding="utf-8" path="C:\Logs"/>
 <eventLog level="Off" />
 <!--Элемент database содержит настройки логирования в БД/>-->
    <!--Атрибут connectionString содержит строку подключения к БД/>-->
    <!--В примере используется драйвер OLEDB для SQLServer (должен быть установлен в системе)/>-->
    <database connectionString="Provider=sqloledb; Server=192.168.0.1; Database=Integrator; User Id=sa; Password=Password123" level="All" table="Logs" dateColumn="Date" levelColumn="Level" machineNameColumn="MachineName" appDomainColumn="AppDomain" requestIdColumn="RequestId" packageNameColumn="PackageName" nodeNameColumn="NodeName" messageColumn="Message" exceptionColumn="Exception" requestColumn="Request" responseColumn="Response"/>
 <internal level="Error" />
</log>
```

%/spoiler%

#### SQL запрос

SQL запрос задается в атрибуте `sqlCommand`.

Если задан атрибут `table`, `sqlCommand` будет проигнорирован.

В качестве устанавливаемых значений в запросе требуется указывать следующие параметры:

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

> **Примечание**: применение атрибута `userNameColumn` и соответствующего ему параметра `:username`  заметно увеличивает время выполнения операции логирования. Рекомендуется не использовать их без необходимости.

%spoiler%Запись полей журнала SQL запросом. Пример раздела `<log>` Integrator.dll.config%spoiler%

``` XML
<!--Параметры логирования задаются в элементе log/>-->
<log>
 <fileSystem level="Off" maxArchiveFiles="30" encoding="utf-8" path="C:\Logs"/>
 <eventLog level="Off" />
 <!--Элемент database содержит настройки логирования в БД/>-->
    <!--Атрибут connectionString содержит строку подключения к БД/>-->
    <!--В примере используется драйвер OLEDB для SQLServer (должен быть установлен в системе)/>-->
    <!--Атрибут sqlCommand задает запрос на запись события журнала/>-->
    <!--При таком способе логирования имеется возможность модификации записей журнала/>-->
    <!--В примере содержимое поля :response будет обрезано до 1000 знаков/>-->
    <database connectionString="Provider=sqloledb; Server=192.168.0.1; Database=Integrator; User Id=sa; Password=Password123" level="All" sqlCommand="insert into Logs (Date, Level, MachineName, AppDomain, RequestId, PackageName, NodeName, Message, Exception, Request, Response) values (:date, :level, :machinename, :appdomain, :requestid, :packagename, :nodename, :message, :exception, :request, SUBSTRING( :response, 0, 999))"/>
    <internal level="Error" />
</log>
```

%/spoiler%

### Запись событий логирования

Параметры записи событий логирования задаются в элементе `configuration/log/internal`:

* **path** — полный или относительный (от рабочего каталога) путь к каталогу журналов. Значение по умолчанию: `Logs`.
* **level** — минимальный уровень журналирования. Значение по умолчанию: `All`.

Имя файла журнала — `logger-internal.log`.
