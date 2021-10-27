# Activation of Software SP Dongle

The following conditions are required for successful dongle activation:

* Installed Guardant driver, version 6.0 or higher.
* GuardantActivationWizard.exe utility supplied with SP dongle (it is included in the distribution kit).
* Programmed template of Guardant SP dongle (*.grdvd file).
* Serial number of the following type: `9XZrwR-XXXXX-7NGDyS-XXXXX-qZGBcW-XXXXX-XoLBXF-xwqwXv-8oEO0o-XXXXX`.
* It is required to provide access to the activation server for the computer used for activation at the following address: `https://sp.guardant.ru` in the process of the dongle initialization.

> **Important**: Guardant SP is distinguished by cryptographic binding to the equipment, namely it is not possible to use the activated dongle on another computer.

To activate Guardant SP dongle, it is required to run GuardantActivationWizard.exe activation wizard and to follow its instructions (If Loginom has been installed with the default settings, the utility can be found in the "Start" menu Windows, Loginom 6 and in `C:\Program Files\BaseGroup\Guardant` folder):

__1.__ Using the "Specify the license file" button, select a path to *.grdvd file. Check the Internet connections settings and press the "Next" button:

![](../images/guardant-sp-activate-1.png)

__2.__ Enter the serial number in the entry field for activation. Press "Next":

![](../images/guardant-sp-activate-2.png)

The wizard will perform the required exchange of information with the dongle driver and activation server. In this respect, the entered serial number is checked, and superencryption of the software dongle file is performed using control values of PC components.

__3.__ If activation is a success, the wizard shows the final dialog window:

![](../images/guardant-sp-activate-3.png)