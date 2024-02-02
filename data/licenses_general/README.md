# License Dongles

Лицензирование продуктов Loginom осуществляется с помощью ключей Guardant. В зависимости от установленной OS ключ имеет разные варианты установки и обновления. Ключи бывают двух основных типов: физический (USB) и програмный (SP). Рассмотрим их подробнее:

* The **Local USB dongle** is a physical dongle that is recommended for one-off Loginom instances on servers with physical access to USB ports.

![Local dongle (personal version) usage](./scheme_1.svg)

![Local dongle (server version) usage](./scheme_2.svg)

* The **Network USB dongle** is a physical dongle that is recommended for a number of Loginom instances on the local network. Deployment of [network dongles server](https://www.guardant.ru/support/users/server/) version 7.0 or higher is required.

![Network dongle usage](./scheme_3.svg)

* The **Software SP dongle** is a software dongle that is recommended for one-off Loginom instances in virtual environments without physical access to USB ports.

> **Important**: An SP dongle is distinguished via cryptographic binding to the equipment, namely it is not possible to reactivate SP dongle after its activation or transfer it to another computer.


Several [physical dongle versions](./case.md) are available to users.

## Activation of Software SP Dongle

It is required to activate SP dongle before use of the platform components.

[Инструкция по активации программного SP-ключа для Widows](../windows/licenses/sp-key-activate.md).

[Инструкция по активации программного SP-ключа для Linux](../linux/licenses/sp-key-activate.md).

## Software SP Dongle Activation for Offline Systems

It is required to activate the software SP dongle before use of platform components, but without net access on the computer of the end user.

[Инструкция по активации программного SP-ключа для Windows offline](../windows/licenses/sp-key-activate-offline.md).

[Инструкция по активации программного SP-ключа для Linux offline](../linux/licenses/sp-key-activate.md).

## Upgrade of Hardware USB Dongles

It is required to reprogram USB dongle in the case of acquisition of additional licenses for Loginom components or options. In that case, remote changing of the dongle memory content with exchange of special codes by e-mail is required.

[Инструкция по обновлению USB-ключей для Windows](../windows/licenses/usb-upgrade.md).