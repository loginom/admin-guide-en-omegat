---
description: Руководство администратора по запуску Loginom 7.0 под Linux в Яндекс.Облаке.
---
# Loginom in Yandex.Cloud

Access to the analytical platform is provided by resources of the Yandex Compute Cloud service. The service enables usage of virtual machines in Yandex.Cloud infrastructure for operation on the Loginom platform. В зависимости от сложности вычислений можно определить необходимое число ядер, объём памяти, размер и количество дисков, а также зону доступности виртуальной машины. As appropriate, these parameters can be changed without a system reinstallation.

[Loginom 7.0 в Яндекс.Облаке](https://cloud.yandex.ru/marketplace/products/loginom/loginom7)

## Запуск

> **Важно**: для использования программы необходимо сначала задать пароль администратора!

Upon deployment and start of the virtual machine in Yandex.Cloud, it is required to perform the following actions:

1. Зайти на удаленную машину по протоколу SSH.
2. Проверить работоспособность служб "loginomd" и "apache2" командами "sudo systemctl status loginomd" и "sudo systemctl status apache2".
3. Перейти в каталог loginom командой "cd /var/opt/loginom" и выполнить команду для смены пароля "sudo ./passwdchange.sh".
4. Перезапустить виртуальную машину командой "sudo reboot".
5. Зайти на веб-страницу приложения Loginom по адресу `http://ваш_ip_или_домен/app/`

   * Логин по умолчанию: "admin";
   * Учетные записи user и service по умолчанию отключены.


## Additional Support

All technical support concerning deployment, startup and usage issues is available by emailing the relevant enquiry to the following address: help@loginom.ru.