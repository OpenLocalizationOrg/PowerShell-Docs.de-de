---
title: Verbesserungen bei der Paketverwaltung in WMF 5.1 (Preview)
ms.date: 2016-07-15
keywords: PowerShell, DSC, WMF
description: 
ms.topic: article
author: jaimeo
contributor: jianyunt, quoctruong
manager: dongill
ms.prod: powershell
ms.technology: WMF
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 615bdf1a82dc5078ee2f37eec70a64e25b42bda2

---

# Verbesserungen bei der Paketverwaltung in WMF 5.1 (Preview) #

## Verbesserungen bei PackageManagement ##
In WMF 5.1 wurden folgende Probleme behoben: 

### Versionsalias

**Szenario**: Angenommen, Sie haben Version 1.0 und 2.0 des Pakets P1 auf Ihrem System installiert und möchten nun die Version 1.0 deinstallieren. Deshalb führen Sie `Uninstall-Package -Name P1 -Version 1.0` aus. Sie erwarten, dass Version 1.0 nach dem Ausführen des Cmdlets deinstalliert wurde. Das Ergebnis ist jedoch, dass Version 2.0 deinstalliert wird.  
    
Dies passiert, weil der `-Version`-Parameter ein Alias für den `-MinimumVersion`-Parameter ist. Wenn PackageManagement ein qualifiziertes Paket mit der Mindestversion 1.0 sucht, wird die neueste Version zurückgegeben. Dieses Verhalten wird normalerweise erwartet, da meistens die neueste Version gefunden werden soll. Für `Uninstall-Package` sollte dies aber nicht gelten.
    
**Lösung**: Der Alias `-Version` wurde in PackageManagement (auch bekannt als OneGet) und PowerShellGet vollständig entfernt. 

### Mehrere Aufforderungen zum Bootstrapping des NuGet-Anbieters

**Szenario**: Wenn Sie `Find-Module` oder `Install-Module` oder andere PackageManagement-Cmdlets erstmals auf Ihrem Computer ausführen, versucht PackageManagement das Bootstrapping des NuGet-Anbieters. Das liegt daran, dass der PowerShellGet-Anbieter auch den NuGet-Anbieter verwendet, um PowerShell-Module herunterzuladen. PackageManagement fordert dann vom Benutzer die Berechtigung zum Installieren des NuGet-Anbieters an. Nachdem der Benutzer für das Bootstrapping „yes“ ausgewählt hat, wird die neueste Version des NuGet-Anbieters installiert. 
    
Wenn jedoch eine ältere Version des NuGet-Anbieters auf Ihrem Computer installiert ist, wird mitunter die ältere NuGet-Version zuerst in die PowerShell-Sitzung geladen (was die Racebedingung in PackageManagement ist). Damit PowerShellGet funktioniert, ist jedoch die neuere Version des NuGet-Anbieters erforderlich. Deshalb wird PackageManagement von PowerShellGet aufgefordert, für den NuGet-Anbieter erneut das Bootstrapping auszuführen. Dies führt zu mehreren Aufforderungen zum Bootstrapping des NuGet-Anbieters.

**Lösung**: In WMF 5.1 lädt PackageManagement die neueste Version des NuGet-Anbieters, um mehrere Aufforderungen zum Bootstrapping des NuGet-Anbieters zu vermeiden.

Es gibt auch eine Umgehung dieses Problems. Löschen Sie dazu manuell die alte Version des NuGet-Anbieters (NuGet-Anycpu.exe), sofern vorhanden, aus „$env:ProgramFiles\PackageManagement\ProviderAssemblies $env:LOCALAPPDATA\PackageManagement\ProviderAssemblies“.


### Unterstützung für PackageManagement auf Computern mit ausschließlichem Intranetzugriff

**Szenario**: Benutzer im Unternehmen haben keinen Zugriff auf das Internet, sondern nur auf das Intranet. Dies wurde von PackageManagement in WMF 5.0 nicht unterstützt.

**Szenario**: In WMF 5.0 hat PackageManagement keine Computer unterstützt, die nur auf das Intranet (aber nicht auf das Internet) zugreifen dürfen.

**Lösung**: In WMF 5.1 können Sie diese Schritte ausführen, um Intranetcomputern das Verwenden von PackageManagement zu erlauben:

1. Laden Sie den NuGet-Anbieter auf einem anderen Computer mit Internetverbindung mit dem Befehl `Install-PackageProvider -Name NuGet` herunter.

2. Suchen Sie den NuGet-Anbieter entweder unter `$env:ProgramFiles\PackageManagement\ProviderAssemblies\nuget` oder `$env:LOCALAPPDATA\PackageManagement\ProviderAssemblies\nuget`.

3. Kopieren Sie die Binärdateien in einen Ordner oder eine Netzwerkfreigabe, auf die der Intranetcomputer zugreifen kann, und installieren Sie den NuGet-Anbieter mit `Install-PackageProvider -Name NuGet -Source <Path to folder>`.


### Verbesserungen bei der Ereignisprotokollierung

Wenn Sie Pakete installieren, ändern Sie den Status des Computers. In WMF 5.1 protokolliert PackageManagement jetzt Ereignisse im Windows-Ereignisprotokoll für die Aktivitäten `Install-Package`, `Uninstall-Package` und `Save-Package`. Das Ereignisprotokoll ist dasselbe wie für PowerShell, d. h. `Microsoft-Windows-PowerShell, Operational`.

### Unterstützung für Standardauthentifizierung

In WMF 5.1 unterstützt PackageManagement das Suchen und Installieren von Paketen aus einem Repository, das Standardauthentifizierung erfordert. Sie können Ihre Anmeldeinformationen für die Cmdlets `Find-Package` und `Install-Package` angeben. Beispiel:

``` PowerShell
Find-Package -Source <SourceWithCredential> -Credential (Get-Credential)
```
### Unterstützung für die Verwendung von PackageManagement hinter einem Proxy

In WMF 5.1 verwendet PackageManagement jetzt die neuen Proxyparameter `-ProxyCredential` und `-Proxy`. Mithilfe dieser Parameter können Sie den Proxy-URL und die Anmeldeinformationen für PackageManagement-Cmdlets angeben. Standardmäßig werden die Proxyeinstellungen des Systems verwendet. Beispiel:

``` PowerShell
Find-Package -Source http://www.nuget.org/api/v2/ -Proxy http://www.myproxyserver.com -ProxyCredential (Get-Credential)
```




<!--HONumber=Oct16_HO1-->


