# Loginom Server Removal

To remove the Loginom Server, it is required to stop the *loginom* service and remove the application using one of the following methods:

* Use the "Programs and Features" window in Windows
* Run the product installer, pressing the**"Remove"** button

![](../../images/server_msi_remove.png)

* Execute the following command in the command line as an administrator:

```cmd
msiexec /x Loginom<Редакця>_7.x.x.msi /qn
```
