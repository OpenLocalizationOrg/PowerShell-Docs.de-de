---
description: 
manager: dongill
ms.topic: article
author: jpjofre
ms.prod: powershell
keywords: powershell,cmdlet,jea
ms.date: 2016-06-22
title: Bereitstellung und Wartung mehrerer Computer
ms.technology: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 784806197a64eb30af1ecea4af55575434ce7b87

---

# Bereitstellung und Wartung mehrerer Computer
Bisher haben Sie JEA mehrmals auf lokalen Systemen bereitgestellt.
Da Ihre Produktionsumgebung aller Wahrscheinlichkeit nach aus mehr als einen Computer besteht, müssen Sie die wichtigen Schritte im Bereitstellungsprozesses kennen, die auf jedem Computer ausgeführt werden müssen.

## Allgemeine Schritte:
1.  Kopieren Sie Ihre Module (mit Rollenfunktionen) auf jeden Knoten.
2.  Kopieren Sie Ihre Sitzungskonfigurationsdateien auf jeden Knoten.
3.  Führen Sie `Register-PSSessionConfiguration` mit Ihrer Sitzungskonfiguration aus.
4.  Speichern Sie eine Kopie Ihrer Sitzungskonfiguration und Toolkits an einem sicheren Ort.
Wenn Sie Änderungen vornehmen, ist es gut, über eine „sichere Informationsquelle“ zu verfügen.

## Beispielskript
Hier sehen Sie ein Beispielskript für die Bereitstellung.
Um dieses Skript in Ihrer Umgebung zu verwenden, müssen Sie die Namen/Pfade echter Dateifreigaben und Module angeben.
```PowerShell
# First, copy the session configuration and modules (containing role capability files) onto a file share you have access to.
Copy-Item -Path 'C:\Demo\Demo.pssc' -Destination '\\FileShare\JEA\Demo.pssc'
Copy-Item -Path 'C:\Program Files\WindowsPowerShell\Modules\SomeModule\' -Recurse -Destination '\\FileShare\JEA\SomeModule'

# Next, author a setup script (C:\JEA\Deploy.ps1) to run on each individual node
    # Contents of C:\JEA\Deploy.ps1
    New-Item -ItemType Directory -Path C:\JEADeploy
    Copy-Item -Path '\\FileShare\JEA\Demo.pssc' -Destination 'C:\JEADeploy\'
    Copy-Item -Path '\\FileShare\JEA\SomeModule' -Recurse -Destination 'C:\Program Files\WindowsPowerShell\Modules' # Remember, Role Capability Files are found in modules
    if (Get-PSSessionConfiguration -Name JEADemo -ErrorAction SilentlyContinue)
    {
        Unregister-PSSessionConfiguration -Name JEADemo -ErrorAction Stop
    }

    Register-PSSessionConfiguration -Name JEADemo -Path 'C:\JEADeploy\Demo.pssc'
    Remove-Item -Path 'C:\JEADeploy' # Don't forget to clean up!

# Now, invoke the script on all of the target machines.
# Note: this requires PowerShell Remoting be enabled on each machine. Enabling PowerShell remoting is a requirement to use JEA as well.
# You may need to provide the "-Credential" parameter if your current user account does not have admin permissions on these machines.
Invoke-Command –ComputerName 'Node1', 'Node2', 'Node3', 'NodeN' -FilePath 'C:\JEA\Deploy.ps1'

# Finally, delete the session configuration and role capability files from the file share.
Remove-Item -Path '\\FileShare\JEA\Demo.pssc'
Remove-Item -Path '\\FileShare\JEA\SomeModule' -Recurse
```
## Ändern von Funktionen
Beim Arbeiten mit vielen Computern ist es wichtig, dass Änderungen auf konsistente Weise eingeführt werden.
Sobald JEA über eine DSC-Ressource verfügt, wird sichergestellt, dass Ihre Umgebung synchronisiert ist.
Bis dahin empfiehlt es sich dringend, eine Masterkopie Ihrer Sitzungskonfigurationen zu speichern und diese nach jeder Änderung erneut bereitzustellen.

## Entfernen von Funktionen:
Um Ihre JEA-Konfiguration von Ihren Systemen zu entfernen, verwenden Sie den folgenden Befehl auf jedem Computer:
```PowerShell
Unregister-PSSessionConfiguration -Name JEADemo
```




<!--HONumber=Oct16_HO1-->


