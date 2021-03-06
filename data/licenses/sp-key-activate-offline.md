# Activation of Software SP Dongle for Offline Systems

 [Activation is possible in offline mode](https://dev.guardant.ru/pages/viewpage.action?pageId=1278815) in the case of network unavailability ([online-mode](./sp-key-activate.md)) for the end user computer at the moment of SP dongle activation. A number of actions are required to that end.

__1.__ Run Guardant Activation Wizard (GuardantActivationWizard.exe), to install the "Offline mode" option by checking the box on the bottom left, then press the "Next" button:

![](./off-scr1.png)

__2.__ Enter the software dongle serial number. Then the program will generate a special file to be sent to the activation server:

![](./off-scr2.png)

__3.__It is required to transfer this file to a computer with net access, to run the Activation Wizard (GuardantActivationWizard.exe), to press the "Specify the license file..." button and to specify the "Files for transfer to the activation server (*toserver)" parameter in the drop-down list of the navigation window in the "Files type:" field:

![](./off-scr3.png)

__4.__ The program will connect with the activation server and generate one more file to be transferred to the end user computer to finish the activation process:

![](./off-scr4.png)


__5.__ It is required to run the Activation Wizard (GuardantActivationWizard.exe) on the end user computer (**Important**: the "Offline mode" option (shown on the bottom left) must not be installed), to press the "Specify the license file..." button and to select the required file in the opened file selection dialog having specified the file type "FIles received from the activation server (*.fromserver)":

![](./off-scr5.png)

__6.__ Press the "Next" button:

![](./off-scr6.png)

__7.__ The activation process is complete:

![](./off-scr7.png)
