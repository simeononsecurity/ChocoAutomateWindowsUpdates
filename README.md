---
title: "Automating Windows Updates with Chocolatey, PSWindowsUpdate, and Startup Scripts"
date: 2020-07-22T17:46:00-05:00
draft: false
---

***Automating Windows Updates with Chocolatey, PSWindowsUpdate, and Startup Scripts***

The script we will be working with today titled "chocoautomatewindowsupdates.ps1":

```
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
choco feature enable -n=allowGlobalConfirmation
choco feature enable -n useFipsCompliantChecksums
choco upgrade all
choco install pswindowsupdate
Add-WUServiceManager -ServiceID 7971f918-a847-4430-9279-4a52d1efe18d -Confirm:$false
Install-WindowsUpdate -MicrosoftUpdate -AcceptAll 
Get-WuInstall -AcceptAll -IgnoreReboot
```

From [Chocolatey](https://chocolatey.org/install), the script below installs the Chocolatey package manager:
```
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

Folowed by a couple prefered Chocolatey configuration changes;
``` 
choco feature enable -n=allowGlobalConfirmation
choco feature enable -n useFipsCompliantChecksums
choco upgrade all
```

Lastly we will install and run [PSWindowsUpdates](https://www.powershellgallery.com/packages/PSWindowsUpdate/2.0.0.4) to install all of the available windows updates.
```
choco install pswindowsupdate
Add-WUServiceManager -ServiceID 7971f918-a847-4430-9279-4a52d1efe18d -Confirm:$false
Install-WindowsUpdate -MicrosoftUpdate -AcceptAll 
Get-WuInstall -AcceptAll -IgnoreReboot
```