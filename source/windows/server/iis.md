# Web-сервер IIS

> Инструкции для IIS 8.5

## Включение компонентов IIS

В командной строке, запущенной от имени администратора:

```cmd
dism /online /enable-feature /FeatureName:IIS-WebServerRole /FeatureName:IIS-WebServer /FeatureName:IIS-WebServerManagementTools /FeatureName:IIS-ManagementScriptingTools
```

## Включение компонентов IIS для Loginom Studio

В командной строке, запущенной от имени администратора:

```cmd
dism /online /enable-feature /FeatureName:IIS-CommonHttpFeatures /FeatureName:IIS-StaticContent /FeatureName:IIS-DefaultDocument /FeatureName:IIS-Performance /FeatureName:IIS-HttpCompressionStatic
```

### Web.config

В каталог `Loginom\Client` надо поместить файл `web.config` следующего содержания:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <system.webServer>
        <httpProtocol>
            <customHeaders>
                <remove name="Cache-Control" />
                <add name="Cache-Control" value="no-cache,must-revalidate" />
            </customHeaders>
        </httpProtocol>
        <staticContent>
            <remove fileExtension=".json" />
            <mimeMap fileExtension=".json" mimeType="application/json" />
            <remove fileExtension=".woff" />
            <mimeMap fileExtension=".woff" mimeType="application/font-woff" />
            <remove fileExtension=".woff2" />
            <mimeMap fileExtension=".woff2" mimeType="application/font-woff2" />
        </staticContent>
    </system.webServer>
</configuration>
```

### Настройка WebSocket Proxy

* Скачиваем и устанавливаем модуль [ARR3.0](https://www.iis.net/downloads/microsoft/application-request-routing#additionalDownloads)
* Скачиваем и устанавливаем модуль [URL Rewrite](https://www.iis.net/downloads/microsoft/url-rewrite)
* Включаем компонент WebSockets:

```cmd
dism /online /enable-feature /FeatureName:IIS-WebSockets
```

* Перезапускаем IIS

```cmd
%windir%\system32\iisreset.exe
```

* Включаем функцию прокси в ARR:

```cmd
%windir%\system32\inetsrv\appcmd set config -section:system.webServer/proxy /enabled:"True"
```

* Файл `web.config` дополняем следующим образом:

```xml
<system.webServer>
...
    <rewrite>
        <rules>
            <rule name="websocket_proxy" patternSyntax="ECMAScript" stopProcessing="false">
                <match url="ws/" />
                <conditions logicalGrouping="MatchAny" trackAllCaptures="false">
                    <add input="{CACHE_URL}" pattern="(ws|http)s?://(.*)" />
                </conditions>
                <action type="Rewrite" url="{C:1}://127.0.0.1:8080/" appendQueryString="false" />
            </rule>
        </rules>
    </rewrite>
</system.webServer>
```

### Создание виртуального каталога для Loginom Studio

Следующая команда добавляет виртуальный каталог `/app` в сайте `Default Web Site`:

```cmd
"%windir%\system32\inetsrv\appcmd.exe" add vdir /app.name:"Default Web Site/" / /path:/app /physicalPath:"%ProgramFiles%\Loginom\Client"
```
