---
title: "Erfassen von Informationen über Computer"
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 9e7b6a2d-34f7-4731-a92c-8b3382eb51bb
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 96204a0ce674cacd5b830f9f8b820ce3e1cbbc20

---

# Erfassen von Informationen über Computer
**Get-WmiObject** ist das wichtigste Cmdlet für allgemeine Systemverwaltungsaufgaben. Alle kritischen Subsystemeinstellungen werden über WMI verfügbar gemacht. Darüber hinaus behandelt WMI Daten als Objekte, die zu Sammlungen eines oder mehrerer Elemente gehören. Da Windows PowerShell ebenfalls mit Objekten arbeitet und über eine Pipeline verfügt, mit der Sie einzelne oder mehrere Objekte auf die gleiche Weise behandeln können, ermöglicht der generische WMI-Zugriff Ihnen, einige erweiterte Aufgaben mit sehr geringem Aufwand auszuführen.

In den folgenden Beispielen wird gezeigt, wie Sie mit **Get-WmiObject** spezifische Informationen für einen beliebigen Computer sammeln. Sie geben den Parameter **ComputerName** mit dem Punkt (**.**) als Wert an, der den lokalen Computer darstellt. Sie können einen Namen oder eine IP-Adresse eines beliebigen Computers angeben, den Sie über WMI erreichen können. Zum Abrufen von Informationen über den lokalen Computer können Sie **-ComputerName** weglassen.

### Auflisten von Desktopeinstellungen
Wir beginnen mit einem Befehl, der Informationen über die Desktops auf dem lokalen Computer erfasst.

```
Get-WmiObject -Class Win32_Desktop -ComputerName .
```

Dadurch werden Informationen über alle Desktops zurückgegeben, ganz gleich, ob sie verwendet werden oder nicht.

> [!NOTE]
> Einige WMI-Klassen geben sehr detaillierte Informationen zurück, die häufig Metadaten über die WMI-Klasse enthalten. Da die meisten dieser Metadateneigenschaften über Namen verfügen, die mit einem doppelten Unterstrich beginnen, können Sie die Eigenschaften mit „Select-Object“ filtern. Geben Sie nur Eigenschaften an, die mit einem Buchstaben beginnen, und verwenden Sie **[a-z]*** als Wert der Eigenschaft. Beispiel:

```
Get-WmiObject -Class Win32_Desktop -ComputerName . | Select-Object -Property [a-z]*
```

Um die Metadaten zu filtern, verwenden Sie einen Pipelineoperator (|), um die Ergebnisse des Befehls „Get-WmiObject“ an **Select-Object -Property [a-z]*** zu senden.

### Auflisten von BIOS-Informationen
Die WMI-Klasse „Win32_BIOS“ gibt kompakte und vollständige Informationen zum System-BIOS auf dem lokalen Computer zurück:

```
Get-WmiObject -Class Win32_BIOS -ComputerName .
```

### Auflisten von Prozessorinformationen
Sie können allgemeine Prozessorinformationen mithilfe der WMI-Klasse **Win32_Processor** abrufen. Allerdings möchten Sie die Daten wahrscheinlich filtern:

```
Get-WmiObject -Class Win32_Processor -ComputerName . | Select-Object -Property [a-z]*
```

Wenn Sie eine Zeichenfolge mit einer allgemeinen Beschreibung der Prozessorfamilie erhalten möchten, lassen Sie die **SystemType**-Eigenschaft zurückgeben:

```
PS> Get-WmiObject -Class Win32_ComputerSystem -ComputerName . | Select-Object -Property SystemType
SystemType
----------
X86-based PC
```

### Auflisten von Informationen zum Computerhersteller und -modell
Informationen zum Computermodell sind auch über **Win32_ComputerSystem** verfügbar. Zum Bereitstellen von OEM-Daten ist keine Filterung der angezeigten Standardausgabe erforderlich:

```
PS> Get-WmiObject -Class Win32_ComputerSystem
Domain              : WORKGROUP
Manufacturer        : Compaq Presario 06
Model               : DA243A-ABA 6415cl NA910
Name                : MyPC
PrimaryOwnerName    : Jane Doe
TotalPhysicalMemory : 804765696
```

Die Ausgabe von Befehlen, die Informationen direkt von bestimmter Hardware zurückgeben, kann nur so gut sein, wie die vorhandenen Daten. Einige Informationen wurden von den Hardwareherstellern nicht ordnungsgemäß konfiguriert und sind daher möglicherweise nicht verfügbar.

### Auflisten installierter Hotfixes
Mit **Win32_QuickFixEngineering** können Sie alle installierten Hotfixes auflisten:

```
Get-WmiObject -Class Win32_QuickFixEngineering -ComputerName .
```

Diese Klasse gibt eine Liste von Hotfixes zurück, die wie folgt aussieht:

```
Description         : Update for Windows XP (KB910437)
FixComments         : Update
HotFixID            : KB910437
Install Date        :
InstalledBy         : Administrator
InstalledOn         : 12/16/2005
Name                :
ServicePackInEffect : SP3
Status              :
```

Wenn Sie eine kompaktere Ausgabe erhalten möchten, können Sie bestimmte Eigenschaften ausschließen. Sie können zwar den **Property**-Parameter von „Get-WmiObject“ verwenden, um nur die **HotFixID** auszuwählen. Dennoch werden weitere Informationen zurückgegeben, da standardmäßig alle Metadaten angezeigt werden:

```
PS> Get-WmiObject -Class Win32_QuickFixEngineering -ComputerName . -Property HotFixID
HotFixID         : KB910437
__GENUS          : 2
__CLASS          : Win32_QuickFixEngineering
__SUPERCLASS     :
__DYNASTY        :
__RELPATH        :
__PROPERTY_COUNT : 1
__DERIVATION     : {}
__SERVER         :
__NAMESPACE      :
__PATH           :
```

Die zusätzlichen Daten werden zurückgegeben, weil der „Property“-Parameter in **Get-WmiObject** die von WMI-Klasseninstanzen zurückgegebenen Eigenschaften, nicht aber das an die Windows PowerShell zurückgegebene Objekt einschränkt. Um die Ausgabe zu verringern, verwenden Sie **Select-Object**:

```
PS> Get-WmiObject -Class Win32_QuickFixEngineering -ComputerName . -Property Hot
FixId | Select-Object -Property HotFixId
HotFixId
--------
KB910437
```

### Auflisten von Informationen zur Betriebssystemversion
Die Klasseneigenschaften **Win32_OperatingSystem** umfassen Informationen zur Version und zum Service Pack. Sie können mit **Win32_OperatingSystem** explizit nur diese Eigenschaften auswählen, um eine Zusammenfassung von Versionsinformationen abzurufen:

```
Get-WmiObject -Class Win32_OperatingSystem -ComputerName . | Select-Object -Property BuildNumber,BuildType,OSType,ServicePackMajorVersion,ServicePackMinorVersion
```

Sie können mit dem ** Property**-Parameter von „Select-Object“ auch Platzhalterzeichen verwenden. Da alle Eigenschaften, die entweder mit **Build** oder **Service Pack** beginnen, hier wichtig sind, können wir das Beispiel auf das folgende Format kürzen:

```
PS> Get-WmiObject -Class Win32_OperatingSystem -ComputerName . | Select-Object -Property Build*,OSType,ServicePack*

BuildNumber             : 2600
BuildType               : Uniprocessor Free
OSType                  : 18
ServicePackMajorVersion : 2
ServicePackMinorVersion : 0
```

### Auflisten der lokalen Benutzer und des Besitzers
Allgemeine lokale Benutzerinformationen (z. B. Anzahl lizenzierter Benutzer, aktuelle Anzahl von Benutzern und Besitzername) können Sie mit einer Auswahl von **Win32_OperatingSystem**-Eigenschaften suchen. Sie können die anzuzeigenden Eigenschaften wie folgt explizit auswählen:

```
Get-WmiObject -Class Win32_OperatingSystem -ComputerName . | Select-Object -Property NumberOfLicensedUsers,NumberOfUsers,RegisteredUser
```

Eine kompaktere Version mit Platzhaltern ist:

```
Get-WmiObject -Class Win32_OperatingSystem -ComputerName . | Select-Object -Property *user*
```

### Abrufen des verfügbaren Speicherplatzes
Um den Speicherplatz und den freien Speicherplatz auf lokalen Laufwerken anzuzeigen, können Sie die WMI-Klasse „Win32_LogicalDisk“ verwenden. Sie brauchen nur Instanzen mit dem „DriveType“-Wert „3“ anzuzeigen. Dieser Wert wird von WMI für Festplatten mit fester Größe verwendet.

```
Get-WmiObject -Class Win32_LogicalDisk -Filter "DriveType=3" -ComputerName .

DeviceID     : C:
DriveType    : 3
ProviderName :
FreeSpace    : 65541357568
Size         : 203912880128
VolumeName   : Local Disk

DeviceID     : Q:
DriveType    : 3
ProviderName :
FreeSpace    : 44298250240
Size         : 122934034432
VolumeName   : New Volume

Get-WmiObject -Class Win32_LogicalDisk -Filter "DriveType=3" -ComputerName . | Measure-Object -Property FreeSpace,Size -Sum | Select-Object -Property Property,Sum
```

### Abrufen von Informationen zur Anmeldesitzung
Allgemeine Informationen zu Benutzern zugeordneten Anmeldesitzungen können Sie über die WMI-Klasse „Win32_LogonSession“ abrufen:

```
Get-WmiObject -Class Win32_LogonSession -ComputerName .
```

### Abrufen des auf einem Computer angemeldeten Benutzers
Mit „Win32_ComputerSystem“ können Sie den auf einem bestimmten Computer angemeldeten Benutzers anzeigen. Dieser Befehl gibt nur den auf dem Systemdesktop angemeldeten Benutzer zurück:

```
Get-WmiObject -Class Win32_ComputerSystem -Property UserName -ComputerName .
```

### Abrufen der lokalen Uhrzeit eines Computers
Sie können die aktuelle lokale Zeit auf einem bestimmten Computer mit der Klasse „WMI-Win32_LocalTime“ abrufen. Da diese Klasse standardmäßig alle Metadaten anzeigt, sollten Sie sie eventuell mit **Select-Object** filtern:

```
PS> Get-WmiObject -Class Win32_LocalTime -ComputerName . | Select-Object -Property [a-z]*

Day          : 15
DayOfWeek    : 4
Hour         : 12
Milliseconds :
Minute       : 11
Month        : 6
Quarter      : 2
Second       : 52
WeekInMonth  : 3
Year         : 2006
```

### Anzeigen des Dienststatus
Zum Anzeigen des Status aller Dienste auf einem bestimmten Computer können Sie das weiter oben beschriebene Cmdlet **Get-Service** lokal verwenden. Für Remotesysteme können Sie die Klasse „WMI-Win32_Service“ verwenden. Wenn Sie die Ergebnisse außerdem mit **Select-Object** auf **Status**, **Name** und **DisplayName** filtern, ist das Ausgabeformat fast identisch mit dem von **Get-Service**:

```
Get-WmiObject -Class Win32_Service -ComputerName . | Select-Object -Property Status,Name,DisplayName
```

Um für die wenigen Dienste mit extrem langen Namen die Anzeige der vollständigen Namen zu ermöglichen, können Sie **Format-Table** mit den Parametern **AutoSize** und **Wrap** verwenden. Dadurch wird die Spaltenbreite optimiert und ermöglicht, dass lange Namen umbrochen und nicht abgeschnitten werden:

```
Get-WmiObject -Class Win32_Service -ComputerName . | Format-Table -Property Status,Name,DisplayName -AutoSize -Wrap
```




<!--HONumber=Oct16_HO1-->


