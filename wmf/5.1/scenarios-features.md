---
title: Neue Szenarien und Features in WMF 5.1 (Preview)
ms.date: 2016-07-13
keywords: PowerShell, DSC, WMF
description: 
ms.topic: article
author: keithb
manager: dongill
ms.prod: powershell
ms.technology: WMF
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 9611a7da48a849b52821ac2890e1ea60441a75e3

---

# Neue Szenarien und Features in WMF 5.1 (Preview) #

> Hinweis: Diese Dokumentation ist vorläufig und kann geändert werden.

## PowerShell-Editionen ##
Ab Version 5.1 steht PowerShell in verschiedenen Editionen zur Verfügung, die unterschiedliche Featuregruppen und Plattformkompatibilität bieten.

- **Desktop-Edition:** Diese Edition basiert auf .NET Framework und bietet Kompatibilität mit Skripts und Modulen für Versionen von PowerShell, die unter Vollversionen von Windows wie Server Core und Windows Desktop ausgeführt werden.
- **Core-Edition:** Diese Edition basiert auf .NET Core und bietet Kompatibilität mit Skripts und Modulen für Versionen von PowerShell, die unter funktionsreduzierten Versionen von Windows wie Nano Server und Windows IoT ausgeführt werden.

**Weitere Informationen zur Verwendung von PowerShell-Editionen**
- [Bestimmen der ausgeführten Version von PowerShell]()
- [Deklarieren Sie ein Modul-Kompatibilität für bestimmte Versionen von PowerShell]()
- [Filtern der Ergebnisse von Get-Module von CompatiblePSEditions]()
- [Ausführung des Skripts zu verhindern, es sei denn, auf eine kompatible Version von PowerShell ausführen]()

## Katalog-Cmdlets  

Dem [Microsoft.PowerShell.Security](https://technet.microsoft.com/en-us/library/hh847877.aspx)-Modul wurden zwei neue Cmdlets hinzugefügt, die Windows-Katalogdateien generieren und überprüfen.  

###New-FileCatalog 
--------------------------------

„New-FileCatalog“ erstellt eine Windows-Katalogdatei für eine Gruppe von Ordnern und Dateien. Diese Katalogdatei enthält die Hashes aller Dateien in bestimmten Pfaden. Benutzer können den Satz von Ordnern zusammen mit den entsprechenden Katalogdateien verteilen, die diese Ordner darstellen. Diese Informationen helfen bei der Überprüfung, ob seit der Erstellung des Katalogs Änderungen an den Ordnern vorgenommen wurden.    

```PowerShell
New-FileCatalog [-CatalogFilePath] <string> [[-Path] <string[]>] [-CatalogVersion <int>] [-WhatIf] [-Confirm] [<CommonParameters>]
```
Die Katalogversionen 1 und 2 werden unterstützt. Version 1 verwendet den SHA1-Hashalgorithmus, um Dateihashes zu erstellen, Version 2 verwendet SHA256. Katalogversion 2 wird weder auf *Windows Server 2008 R2* noch auf *Windows 7* unterstützt. Daher sollten Sie die Katalogversion 2 mit *Windows 8*, *Windows Server 2012* und späteren Betriebssystemen verwenden.  

![](../images/NewFileCatalog.jpg)

Dadurch wird die Katalogdatei erstellt. 

![](../images/CatalogFile1.jpg)  

![](../images/CatalogFile2.jpg) 

Melden Sie sich mit dem Cmdlet [Set-AuthenticodeSignature](https://technet.microsoft.com/library/hh849819.aspx) an, um die Integrität der Katalogdatei zu überprüfen (im obigen Beispiel: „pester.cat“).   


###Test-FileCatalog 
--------------------------------

„Test-FileCatalog“ überprüft den Katalog, der einen Satz von Ordnern darstellt. 

```PowerShell
Test-FileCatalog [-CatalogFilePath] <string> [[-Path] <string[]>] [-Detailed] [-FilesToSkip <string[]>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

![](../images/TestFileCatalog.jpg)

Dieses Cmdlet vergleicht alle Dateihashes und deren im *Katalog* enthaltenen relativen Pfade mit denen auf dem *Datenträger*. Wenn das Cmdlet eine Unstimmigkeit zwischen den Dateihashes und den Pfaden entdeckt, gibt es den Status *ValidationFailed* zurück. Benutzer können alle diese Informationen mithilfe des Parameters *-Detailed* abrufen. Dieser Parameter zeigt in der Eigenschaft *Signature* auch den Signierstatus des Katalogs an, was dem Aufruf des Cmdlets [Get-AuthenticodeSignature](https://technet.microsoft.com/en-us/library/hh849805.aspx) in der Katalogdatei entspricht. Benutzer können außerdem jede Datei während der Überprüfung mithilfe des Parameters *-FilesToSkip* überspringen. 


## Die Datei „ModuleAnalysisCache“ ##
Ab Version 5.1 bietet PowerShell Kontrolle über die Datei, die zum Zwischenspeichern von Daten zu einem Modul verwendet wird, z. B. der Befehle, die es exportiert.

Standardmäßig wird dieser Cache in der Datei `${env:LOCALAPPDATA}\Microsoft\Windows\PowerShell\ModuleAnalysisCache` gespeichert.
Der Cache wird in der Regel beim Start der Suche nach einem Befehl gelesen und einige Zeit nach dem Import des Moduls in einem Hintergrundthread geschrieben.

Um den Standardspeicherort des Caches zu ändern, legen Sie die Umgebungsvariable `$env:PSModuleAnalysisCachePath` vor dem Starten von PowerShell fest. Änderungen an dieser Umgebungsvariablen betreffen nur untergeordnete Prozesse. Der Wert muss einen vollständigen Pfad (samt Dateiname) definieren, in dem PowerShell die Berechtigung zum Erstellen und Schreiben von Dateien hat. Zum Deaktivieren des Dateicaches kann dieser Wert auf einen ungültigen Speicherort festgelegt werden. Beispiel:

```PowerShell
$env:PSModuleAnalysisCachePath = 'nul'
```

Hiermit wird der Pfad auf ein ungültiges Gerät festgelegt. Wenn PowerShell nicht in den Pfad schreiben kann, wird keine Fehlermeldung zurückgegeben. Mithilfe eines Ablaufverfolgungstools können Sie jedoch einen Fehlerbericht anzeigen:

```PowerShell
Trace-Command -PSHost -Name Modules -Expression { Import-Module Microsoft.PowerShell.Management -Force }
```

Bei der Ausgabe aus dem Cache sucht PowerShell nach Modulen, die nicht mehr vorhanden sind, um einen unnötig großen Cache zu vermeiden.
Mitunter sind diese Überprüfungen nicht wünschenswert, weshalb Sie sie deaktivieren können, indem Sie Folgendes festlegen:

```PowerShell
$env:PSDisableModuleAnalysisCacheCleanup = 1
```

Das Festlegen dieser Umgebungsvariablen wird im aktuellen Prozess sofort wirksam.

##Angeben der Modulversion

In WMF 5.1 verhält sich `using module` genauso wie andere modulbezogene Konstruktionen in PowerShell. Zuvor gab es keine Möglichkeit, eine bestimmte Modulversion anzugeben. Wenn mehrere Versionen vorhanden waren, führte dies zu einem Fehler.


In WMF 5.1:

* Sie können die `ModuleSpecification` [Hashtabelle](https://msdn.microsoft.com/en-us/library/jj136290(v=vs.85).aspx) verwenden. Diese Hashtabelle hat das gleiche Format wie `Get-Module -FullyQualifiedName`.

**Beispiel:** `using module @{ModuleName = 'PSReadLine'; RequiredVersion = '1.1'}`

* Wenn mehrere Versionen des Moduls vorhanden sind, verwendet PowerShell **dieselbe Auflösungslogik** wie `Import-Module` und gibt keinen Fehler zurück, was dem Verhalten von `Import-Module` und `Import-DscResource` entspricht.


##Verbesserungen an Pester
In WMF 5.1 wurde die in PowerShell mitgelieferte Pester-Version von 3.3.5 auf 3.4.0 aktualisiert und um eine „commit“-Funktion ergänzt, die die Funktionsweise von Pester auf Nano Server verbessert (https://github.com/pester/Pester/pull/484/commits/3854ae8a1f215b39697ac6c2607baf42257b102e). 

Die Änderungen, die in Version 3.4.0 hinsichtlich der Version 3.3.5 gemacht wurden, können Sie in der Datei „ChangeLog.md“ unter „https://github.com/pester/Pester/blob/master/CHANGELOG.md“ einsehen.



<!--HONumber=Oct16_HO1-->


