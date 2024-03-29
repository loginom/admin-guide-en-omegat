# Loginom Integrator Configuration

Loginom Integrator is configured by means of the xml [Web.config](https://ru.wikipedia.org/wiki/Web.config) file. The file is located by default - `%ProgramFiles%\BaseGroup\Loginom 6\Integrator`.

## Connection with the Loginom Server

Loginom Integrator can support connections with several Loginom Server instances simultaneously.

It is required to add the `server` element for each used Loginom Server instance in the `configuration/loginom` element with the following attributes:

* **address** - network address of the Loginom Server host. The required attribute. Default value: localhost.
* **port** - TCP [server port](../server/setup.md#parametry-loginom-server). Значение по умолчанию: 4580.
* **userName** - account name with service login rights. If the attribute is not specified, it is required to use the service user login and password.
* **password** - account password with service login rights. Default blank row.
* **reserved** - the attribute with the "true" value marks the server as the standby unit.

Loginom Integrator maintains the information on operability of servers and packages published in them.

Upon receipt of request, the server is randomly selected among the available primary (not standby) servers providing the required package.

If there are no such servers, it is randomly selected among the available standby servers.

## Working Directory

The working directory contains the temporary data required for the application operation.

The default path to the directory - `%ALLUSERSPROFILE%\BaseGroup\Loginom 6\Integrator`.  It is possible to redefine the value in the `workDir` attribute of the `configuration/loginom` element. The value can be both a full and a relative (from the configuration file) path.

If several Loginom Integrator instances are used, the working directories must be different.

## Logging

The logging parameters must be specified in the `configuration/log` element.

It is possible to enable the following modes:

* **fileSystem** - write to file;
* **eventLog** - write to Windows event log;
* **database** - write to database;
* **internal** - write to logging events file (for example, errors while writing logs to the database).

It is possible to assign the minimum level of detail to each mode:

* **All** - all events;
* **Trace** - trace;
* **Debug** - debug;
* **Info** - info;
* **Warn** - warnings;
* **Error**- errors;
* **Fatal** - fatal errors;
* **Off** - logging off.

In the case of default setup, writing to file with the Info detailing level is enabled. Logs are recorded in the `%ALLUSERSPROFILE%\BaseGroup\Loginom 6\Integrator\Logs\` directory.

> **Note**: Request texts and service responses are written in the log providing the `Trace` logging level or higher.

### Write to File

Parameters of writing to file are specified in the `configuration/log/fileSystem` element:

* **path** - full or relative (from the working directory) path to the log directory. Default value: `Logs`.
* **maxArchiveFiles** - maximum number of archive logs. In the case of "0" value number of files is not limited. Default value: 0;
* **level** - minimum logging level. Default value: `All`.

Log file name - `log.log`.

New files are created once a day, old log files receive names of the `log.yyyy-MM-dd.log` type.

### Write to Windows Events Log

Parameters of writing to Windows events log are specified in the `configuration/log/eventLog` element:

* **level** - minimum logging level. Default value: `All`.

Event source in Windows log: `Loginom Integrator`.

### Write to Database

Parameters of writing to database are specified in the `configuration/log/database` element:

* **connectionString** - database connection string. The required attribute.
* **level** - minimum logging level. Default value: `All`.

Target fields for the event data record can be specified in two ways: it is possible to bind the log fields to the table fields or to write SQL query.

#### Binding to the table fields

It is required to set the following attributes for binding:

* **table** - table name;
* **dateColumn** - field name for the time record;
* **levelColumn** - field name for the event level record;
* **machineNameColumn** - field name for NetBIOS record of the host name on which Integrator runs;
* **userNameColumn** - field name for the user record on whose behalf the Integrator process was initiated;
* **appDomainColumn** - field name for the application domain identifier record;
* **requestIdColumn** - field name for the unique identifier request record;
* **packageNameColumn** - field name for the executed package name record;
* **nodeNameColumn** - field name for the executed node name record;
* **messageColumn** - field name for the event text record;
* **exceptionColumn** - field name for the error text record;
* **requestColumn** - field name for the text record of the request to the web service;
* **responseColumn** - field name for the text record of the the web service response.

Attributes with field names can be absent or contain blank values. In this regard, corresponding data must not be logged.

%spoiler%Example of the script creation of the table to store logs in MS SQL:%spoiler%

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

%spoiler%Binding of the log fields to the table fields. Example of `<log>` web.config:%spoiler% section

```XML
<!--The logging parameters are set in the following item: log/>-->
<log>
	<fileSystem level="Off" maxArchiveFiles="30" encoding="utf-8" path="C:\Logs"/>
	<eventLog level="Off" />
	<!--Database item contains the logging settings in the DB/>-->
    <!--ConnectionString attribute contains the string of connection to the DB/>-->
    <!--OLEDB driver for SQLServer is used in the example (it must be installed in the system)/>-->
    <database connectionString="Provider=sqloledb; Server=192.168.0.1; Database=Integrator; User Id=sa; Password=Password123" level="All" table="Logs" dateColumn="Date" levelColumn="Level" machineNameColumn="MachineName" appDomainColumn="AppDomain" requestIdColumn="RequestId" packageNameColumn="PackageName" nodeNameColumn="NodeName" messageColumn="Message" exceptionColumn="Exception" requestColumn="Request" responseColumn="Response"/>
	<internal level="Error" />
</log>
```

%/spoiler%

#### SQL Query

An SQL query is specified in the `sqlCommand` attribute.

If the `table` attribute is specified, `sqlCommand` will be ignored.

It is required to specify the following parameters in the query as set values:

* **:date** - event time;
* **:level** - event level;
* **:machinename** - NetBIOS name of the host on which Integrator runs;
* **:username** - name of the user on whose behalf the Integrator process was initiated;
* **:appdomain** - application domain identifier;
* **:requestid** - unique request identifier;
* **:packagename** - name of the executed package;
* **:nodename** - name of the executed node;
* **:message** - event text;
* **:exception** - error text;
* **:request** - text of the request to the web service;
* **:response** - text of the web service response.

> **Note**: Use of `userNameColumn` attribute and its corresponding `:username` parameter significantly increases the logging operation execution time. It is recommended not to use them unless necessary.

%spoiler%Write the log fields by the SQL query. Example of `<log>` web.config:%spoiler% section

```XML
<!--The logging parameters are set in the following item: log/>-->
<log>
	<fileSystem level="Off" maxArchiveFiles="30" encoding="utf-8" path="C:\Logs"/>
	<eventLog level="Off" />
	<!--Database item contains the logging settings in the DB/>-->
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

* **path** - full or relative (from the working directory) path to the log directory. Default value: `Logs`.
* **level** - minimum logging level. Default value: `All`.

Log file name - `logger-internal.log`.
