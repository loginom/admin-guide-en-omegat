# Loginom in Yandex.Cloud

<p><iframe allowfullscreen="" frameborder="0" height="472" src="https://www.youtube.com/embed/rOYXRR-Lzow" width="835"></iframe></p>


Access to the analytical platform is provided by resources of the Yandex Compute Cloud service. The service enables usage of virtual machines in Yandex.Cloud infrastructure for operation on the Loginom platform. Depending on the complexity of computations, it is possible to define the required number of cores, storage capacity, size and amount of disks and accessibility zone of the virtual machine. As appropriate, these parameters can be changed without a system reinstallation.

[Loginom Team in Yandex.Cloud](https://loginom.ru/yandex-cloud-redirect)

## Start Operation

Upon deployment and start of the virtual machine in Yandex.Cloud, it is required to perform the following actions:

1. Go to the remote desktop using RDP protocol

2. Open `ReadMe.txt` file (the shortcut is on the desktop). The file contains the following information:

* Loginom Web Admin Password: "admin" user password – administrator of Loginom web application
* Password for system user .\loginom: password of the Windows OS user, on whose behalf Loginom Server services and web servers are started and used

If Loginom Studio start failed, it is required to use the [manual](../windows/server/setup.md) and check availability of the started services.
`Web server start` and `Loginom server start` shortcuts (on the desktop) enable start of the services required for Loginom Studio operation.

## Operational Continuity

There are additional shortcuts on the desktop in RDP session:

1. Ярлык `GuardantActivationWizard` — приложение для активации программного SP-ключа ПО Loginom, [инструкция](../licenses/sp-key-activate.md) по активации находится в файле "Активация SP-ключей".

2. Ярлык `Задать пароль admin` — запускает скрипт принудительного задания и обновления пароля пользователя `admin`. A new password can be required, for example, in the case of current password loss.

3. Ярлык `Loginom Studio` — открывает страницу c веб-приложением Loginom Studio. Google Сhrome is the recommended browser.

> Attention: The Loginom version located in the software image has a limited period of effect, and you will need to request a licence renewal for effective operational continuity of the software by sending an e-mail to the following address: help@loginom.ru.

## Additional Support

All technical support concerning deployment, startup and usage issues is available by emailing the relevant enquiry to the following address: help@loginom.ru.

