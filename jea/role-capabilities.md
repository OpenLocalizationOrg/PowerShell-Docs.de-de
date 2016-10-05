---
description: 
manager: dongill
ms.topic: article
author: jpjofre
ms.prod: powershell
keywords: powershell,cmdlet,jea
ms.date: 2016-06-22
title: Rollenfunktionen
ms.technology: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: a3dd4a217f5b1fd80e97adf802c65073ca015bbc

---

# Rollenfunktionen

## Übersicht
Im vorherigen Abschnitt haben Sie erfahren, dass das Feld RoleDefinitions definiert, welche Gruppen Zugriff auf bestimmte Rollenfunktionen haben.
Möglicherweise haben Sie sich gefragt, was denn Rollenfunktionen sind.
Diese Frage wird im vorliegenden Abschnitt beantwortet.  

## Einführung in PowerShell-Rollenfunktionen
PowerShell-Rollenfunktionen definieren die Aufgaben, die ein Benutzer auf einem JEA-Endpunkt ausführen darf.
Sie stellen eine Whitelist mit sichtbaren Befehlen und Anwendungen sowie weiteren nutzbaren Elementen bereit.
Rollenfunktionen werden durch Dateien mit der Erweiterung PSRC definiert.

## Inhalte von Rollenfunktionen
Zunächst untersuchen und bearbeiten wir die Datei mit den Demorollenfunktionen, die Sie bereits verwendet haben.
Stellen Sie sich folgende Situation vor: Sie haben Ihre Sitzungskonfiguration in der gesamten Umgebung bereitgestellt und erhalten jetzt Feedback, dass Sie die Funktionen ändern müssen, die Benutzern verfügbar gemacht werden.
Operatoren müssen Computer neu starten können und sie möchten auch in der Lage sein, Informationen zu Netzwerkeinstellungen abzurufen.
Darüber hinaus hat das Sicherheitsteam Sie darüber informiert, dass es nicht akzeptabel ist, Benutzern die uneingeschränkte Ausführung von Restart-Service zu ermöglichen.
Sie müssen die Dienste einschränken, die von Operatoren neu gestartet werden dürfen.

Um diese Änderungen durchzuführen, führen Sie die PowerShell ISE als Administrator aus, und öffnen Sie die folgende Datei:

```
C:\Program Files\WindowsPowerShell\Modules\Demo_Module\RoleCapabilities\Maintenance.psrc
```

Suchen Sie die folgenden Zeilen in der Datei, und aktualisieren Sie sie:

```PowerShell
# OLD: VisibleCmdlets = 'Restart-Service'
VisibleCmdlets = 'Restart-Computer',
                 @{
                     Name = 'Restart-Service'
                     Parameters = @{ Name = 'Name'; ValidateSet = 'Spooler' }
                 },
                 'NetTCPIP\Get-*'

# OLD: VisibleExternalCommands = 'Item1', 'Item2'
VisibleExternalCommands = 'C:\Windows\system32\ipconfig.exe'
```

Dies enthält einige interessante Beispiele:

1.  Sie haben Restart-Service eingeschränkt, sodass Operatoren Restart-Service nur mit dem Parameter „-Name“ verwenden und als Argument für diesen Parameter nur „Spooler“ bereitstellen dürfen.
Wenn gewünscht, können Sie mithilfe eines regulären Ausdrucks unter Verwendung von ValidatePattern anstelle von ValidateSet auch die Argumente einschränken.

2.  Sie haben alle Befehle mit dem Verb „Get“ aus dem NetTCPIP-Modul verfügbar gemacht.
Da Get-Befehle üblicherweise den Systemstatus nicht ändern, ist dies ein relativ sicheres Vorgehen.
Nichtsdestoweniger sollten Sie jeden Befehl, den Sie über JEA verfügbar machen, sehr sorgfältig überprüfen.

3.  Sie haben mithilfe von VisibleExternalCommands eine ausführbare Datei (IPCONFIG) verfügbar gemacht.
Mit diesem Feld können Sie auch vollständige PowerShell-Skripts verfügbar machen.
Es ist wichtig, immer den vollständigen Pfad zu externen Pfaden bereitzustellen, um sicherzustellen, dass nicht versehentlich ein (möglicherweise schädliches) Programm mit ähnlichem Namen ausgeführt wird, das sich im Benutzerpfad befindet.

Speichern Sie die Datei, und stellen Sie dann erneut eine Verbindung mit dem Demoendpunkt her, um zu bestätigen, dass die Änderungen funktioniert haben.

```PowerShell
Enter-PSSession -ComputerName . -ConfigurationName JEADemo2 -Credential $NonAdminCred
Get-Command
```
Da Sie nur die Rollenfunktionsdatei geändert haben, müssen Sie die Sitzungskonfiguration nicht erneut registrieren.
PowerShell findet automatisch die aktualisierten Rollenfunktionen, wenn ein Benutzer eine Verbindung herstellt.
Da Rollenfunktionen beim Start der Sitzung geladen werden, sind vorhandene Sitzungen von Aktualisierungen der Rollenfunktionsdateien nicht betroffen.

Bestätigen Sie jetzt, dass Sie den Computer neu starten können, indem Sie Restart-Computer mit dem Parameter „-WhatIf“ ausführen (es sei denn, Sie möchten den Computer tatsächlich neu starten).

```PowerShell
Restart-Computer -WhatIf
```

Bestätigen, dass Sie „ipconfig“ ausführen können.

```PowerShell
ipconfig
```

Zum Schluss bestätigen Sie, dass Restart-Service nur für den Spoolerdienst funktioniert.

```PowerShell
Restart-Service Spooler # This should work
Restart-Service WSearch # This should fail
```

Beenden Sie die Sitzung, wenn Sie fertig sind.

```PowerShell
Exit-PSSession
```

## Erstellen von Rollenfunktionen
Im nächsten Abschnitt erstellen Sie einen JEA-Endpunkt für AD-Helpdeskbenutzer.
Zur Vorbereitung erstellen Sie zunächst eine leere Rollenfunktionsdatei, die in diesem Abschnitt ausgefüllt werden soll.
Rollenfunktionen müssen in einem gültigen PowerShell-Modul innerhalb eines Ordners namens „RoleCapabilities“ erstellt werden, damit sie beim Start einer Sitzung aufgelöst werden können.

Bei PowerShell-Modulen handelt es sich im Wesentlichen um Pakete mit PowerShell-Funktionalitäten.
Sie können PowerShell-Funktionen, -Cmdlets, -DSC-Ressourcen, -Rollenfunktionen und vieles mehr enthalten.
Sie können Informationen zu Modulen erhalten, indem Sie `Get-Help about_Modules` in einer PowerShell-Konsole ausführen.

Um ein neues PowerShell-Modul mit einer leeren Rollenfunktionsdatei zu erstellen, führen Sie folgende Befehle aus:  

```PowerShell
# Create a new folder for the module.
New-Item -Path 'C:\Program Files\WindowsPowerShell\Modules\Contoso_AD_Module' -ItemType Directory

# Add a module manifest to contain metadata for this module.
New-ModuleManifest -Path 'C:\Program Files\WindowsPowerShell\Modules\Contoso_AD_Module\Contoso_AD_Module.psd1' -RootModule Contoso_AD_Module.psm1

# Create a blank script module. You'll use this for custom functions in the next section.
New-Item -Path 'C:\Program Files\WindowsPowerShell\Modules\Contoso_AD_Module\Contoso_AD_Module.psm1' -ItemType File

# Create a RoleCapabilities folder in the Contoso_AD_Module folder. PowerShell expects Role Capabilities to be located in a "RoleCapabilities" folder within a module.
New-Item -Path 'C:\Program Files\WindowsPowerShell\Modules\Contoso_AD_Module\RoleCapabilities' -ItemType Directory

# Create a blank Role Capability in your RoleCapabilities folder. Running this command without any additional parameters just creates a blank template.
New-PSRoleCapabilityFile -Path 'C:\Program Files\WindowsPowerShell\Modules\Contoso_AD_Module\RoleCapabilities\ADHelpDesk.psrc'
```

Herzlichen Glückwunsch! Sie haben eine leere Rollenfunktionsdatei erstellt.
Diese Datei wird im nächsten Abschnitt verwendet.

## Wichtige Konzepte
**Rollenfunktionen (PSRC)**: Eine Datei, die die Aufgaben definiert, die ein Benutzer auf einem JEA-Endpunkt ausführen darf.
Sie stellt eine Whitelist mit sichtbaren Befehlen und Konsolenanwendungen sowie weiteren nutzbaren Elementen bereit.
Damit PowerShell die Rollenfunktionen erkennen kann, müssen Sie diese in einem gültigen PowerShell-Modul im Ordner „RoleCapabilities“ speichern.

**PowerShell-Modul**: Ein Paket mit PowerShell-Funktionalität.
Es kann PowerShell-Funktionen, -Cmdlets, -DSC-Ressourcen, -Rollenfunktionen und vieles mehr enthalten.
PowerShell-Module müssen in einem Pfad in `$env:PSModulePath` gespeichert werden, damit sie automatisch geladen werden können.




<!--HONumber=Oct16_HO1-->


