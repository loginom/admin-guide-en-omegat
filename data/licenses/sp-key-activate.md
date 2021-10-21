# Activation of Software SP Dongle

The following conditions are required for successful dongle activation:

* Installed Guardant driver, version 6.0 or higher.
* Утилита GuardantActivationWizard.exe, поставляющаяся в комплекте с SP-ключом (входит в состав дистрибутива Loginom).
* Programmed template of Guardant SP dongle (*.grdvd file).
* Serial number of the following type: `9XZrwR-XXXXX-7NGDyS-XXXXX-qZGBcW-XXXXX-XoLBXF-xwqwXv-8oEO0o-XXXXX`.
* Компьютер, где будет выполняться активация, в процессе инициализации ключа должен иметь доступ к серверу активации по адресу `https://sp.guardant.ru`.

> **Important**: Guardant SP is distinguished by cryptographic binding to the equipment, namely it is not possible to use the activated dongle on another computer.

Чтобы активировать ключ Guardant SP, запустите мастер активации GuardantActivationWizard.exe и следуйте его указаниям (если установка Loginom выполнена с настройками по умолчанию, то утилиту можно найти в меню Windows "Пуск", пункт Loginom 6, а также в папке `C:\Program Files\BaseGroup\Guardant`):

__1.__ Using the "Specify the license file" button, select a path to *.grdvd file. Check the Internet connections settings and press the "Next" button:

![](../images/guardant-sp-activate-1.png)

__2.__ Enter the serial number in the entry field for activation. Press "Next":

![](../images/guardant-sp-activate-2.png)

The wizard will perform the required exchange of information with the dongle driver and activation server. При этом происходит проверка введенного серийного номера, а также перешифрование файла программного ключа с использованием контрольных значений комплектующих компьютера.

__3.__ If activation is a success, the wizard shows the final dialog window:

![](../images/guardant-sp-activate-3.png)