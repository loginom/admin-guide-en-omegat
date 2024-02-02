# Configuration

Путь к файлу конфигурации Loginom Studio: `%ProgramFiles%\Loginom\Client\server.json`

URL connection to [Loginom server](../windows/server/README.md) is set in the configuration file.

Default values:

```json
{
    "wsport": null,
    "host": null
}
```

Результирующий url:

* http: `ws://web-server-host:8080`
* https: `wss://web-server-host:8443`

Encryption of `ws` protocol is defined by the availability of `http` encryption.

## Parameters

### host

Loginom server host address. In the case of the `null` value the web server host address is used.

```json
{
    "wsport": null,
    "host": "lg.domain.org"
}
```

Результирующий url:

* http: `ws://lg.domain.org:8080`
* https: `wss://lg.domain.org:8443`

### wsport

Port for connection using `websocket` protocol. In case of the `null` value, `8080` port is used.

```json
{
    "wsport": 9999,
    "host": null
}
```

Результирующий url:

* http: `ws://web-server-host:9999`
* https: insecure `websocket` connections are forbidden.

### wssport

Port for connection using `websocket secure` protocol. In the case of the `null` value, `8443` port is used.

```json
{
    "wssport": 9443,
    "host": null
}
```

Результирующий url:

* `wss://web-server-host:9443`

### wsproxy

Connection to Loginom server by means of websocket proxy. In that case `host` and `wsport|wssport` will be equal to the host address web server port.

```json
{
    "wsproxy": true,
    "host": null
}
```

Результирующий url:

* `ws://web-server-host:443/ws/`

### path

Connection path to WebSocket while `wsproxy` using. By default `ws/`.

```json
{
    "wsproxy": true,
    "path": "any/",
    "host": null
}
```

Результирующий url:

* `wss://web-server-host:443/any/`

### secure

Always use encryption when connecting to the Loginom server.

If the parameter is not specified, in the case of connection to the web server `http` connection to Loginom server will be performed without encryption.

If the parameter is specified and set in `true`, connection will be always performed with encryption — using `websocket secure` protocol for `wssport` port.

```json
{
    "host": "loginom-server-host",
    "secure": true
}
```

Результирующий url:

* without wsproxy: `wss://loginom-server-host:8443`
* with wsproxy: `wss://web-server-host:443/ws/`