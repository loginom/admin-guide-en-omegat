# Studio

It is a client web application designated for the design of processing workflows, data visualization, server configuration, and user rights management. Data processing is performed by [Loginom Server](../windows/server/README.md).

Studio is the main working space for analysts and users implementing [user interface](https://help.loginom.ru) of the platform operation.

The following browsers are supported:

* [Chrome](https://www.google.ru/chrome/) не ниже 84.0
* [Chrome Android](https://play.google.com/store/apps/details?id=com.android.chrome&hl=en&gl=US)  не ниже 84.0
* [Firefox](https://www.mozilla.org/en-US/firefox/organizations/) не ниже 79.0
* [Opera](http://www.opera.com/ru) не ниже 70.0
* [Safari](https://www.apple.com/ru/safari/) не ниже 14.1
* [Edge](https://www.microsoft.com/ru-ru/windows/microsoft-edge) не ниже 84.0
* прочие браузеры на базе [Chromium](https://www.chromium.org/getting-involved/download-chromium/) не ниже 84.0

Parameters for connection to the Loginom Server are set in the [configuration file](./config.md).

## Interaction of Components

Studio enables data exchange with Loginom Server using [websocket](https://ru.wikipedia.org/wiki/WebSocket) protocol. Connection is provided using one of the following two methods - directly from Loginom server or using websocket proxy configured on the web server.

Websocket proxy enables access to Loginom Server via http(s) port of the web server that facilitates the firewall configuration.

To enable wsproxy on the imbedded web server, it is sufficient to select "Use WebSocket proxy" [in the course of installation](../windows/server/setup.md#parametry-web-servera-apache-httpd), IIS will require a more complicated [configuration](../windows/server/iis.md#nastroyka-websocket-proxy).

In the case of the default installation, websocket proxy is disabled.

### Without wsproxy

![](../images/server-wsproxy-no.svg)

* Browser is connected to the web server using `http` protocol; it enables Loginom Studio loading:
   * `http://web-server-host:80/app` - URL connections if http encryption is not enabled;
   * `https://web-server-host:443/app` - URL connections if http encryption is enabled.
* [server.json](../studio/config.md) configuration enables URL formation for connection to Loginom server;
* Loginom Studio enables connection to the Loginom server host using `websocket` protocol:
   * `ws://loginom-server-host:8080/ws` - URL connections if websocket encryption [is not enabled](../windows/server/setup.md#parametry-loginom-server). In the case of http encryption, connection is forbidden.
   * `wss://loginom-server-host:8443/ws` - URL connections if websocket encryption [is enabled](../windows/server/setup.md#parametry-loginom-server).

### With wsproxy

![](../images/server-wsproxy-yes.svg)

* Browser is connected to the web server using `http` protocol; it enables Loginom Studio loading:
   * `http://web-server-host:80/app` - URL connections if http encryption is not enabled;
   * `https://web-server-host:443/app` - URL connections if http encryption is enabled.
* [server.json](../studio/config.md) configuration enables URL formation for connection to Loginom server;
* Loginom Studio enables connection to the web server host using `websocket` protocol:
   * `ws://web-server-host:80/ws` - URL connections if neither http, nor websocket encryption is enabled;
   * `wss://web-server-host:443/ws`  - URL connections if http or websocket encryption is enabled.
* The web server enables connection to the Loginom server host using `websocket` protocol and retransfer to it of connection traffic with Loginom Studio:
   * `ws://loginom-server-host:8080/ws` - URL connections if websocket encryption [is disabled](../windows/server/setup.md#parametry-loginom-server);
   * `wss://loginom-server-host:8443/ws` - URL connections if websocket encryption [is enabled](../windows/server/setup.md#parametry-loginom-server).