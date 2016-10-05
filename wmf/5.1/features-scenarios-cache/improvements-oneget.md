---
title: Verbesserungen an OneGet in WMF 5.1 (Vorschau)
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
ms.openlocfilehash: 1d0bd545b52ef56045f2ec740b05c4e0fd93bb67

---

# Verbesserungen an OneGet
WMF 5.1 bietet verschiedene Korrekturen und Verbesserungen, um einige Probleme bei der Benutzererfahrung in der WMF-Version 5.0 zu beheben. 

##„-version“-Alias entfernt.

**Szenario**: Angenommen, Sie haben Version 1.0 und 2.0 des Pakets P1 auf Ihrem System installiert und möchten nun die Version 1.0 deinstallieren. Deshalb führen Sie „uninstall-package -name P1 -version 1.0“ aus. Sie erwarten, dass Version 1.0 nach dem Ausführen des Cmdlets deinstalliert wurde. Das Ergebnis ist jedoch, dass Version 2.0 deinstalliert wird. 
    
Dies passiert, weil der „-version“-Parameter ein Alias für den „-minimumversion“-Parameter ist. Wenn OneGet ein qualifiziertes Paket mit der Mindestversion 1.0 sucht, wird die neueste Version zurückgegeben. Dieses Verhalten wird normalerweise erwartet, da meistens die neueste Version gefunden werden soll. Für den Fall „uninstall-package“ sollte dies aber nicht gelten.
    
**Lösung**: In WMF 5.1 wurde der „-version“-Alias aus OneGet und PowerShellGet vollständig entfernt. 

##Mehrere Aufforderungen zum Bootstrapping des NuGet-Anbieters

**Szenario**: Wenn Sie „Find-Module“ oder „Install-Module“ oder andere OneGet-Cmdlets erstmals auf Ihrem Computer ausführen, versucht OneGet das Bootstrapping des NuGet-Anbieters. Das liegt daran, dass der PowerShellGet-Anbieter auch den NuGet-Anbieter verwendet, um PowerShell-Module herunterzuladen. OneGet fordert dann vom Benutzer die Berechtigung zum Installieren des NuGet-Anbieters an. Nachdem der Benutzer für das Bootstrapping „yes“ ausgewählt hat, wird die neueste Version des NuGet-Anbieters installiert. 
    
Wenn jedoch eine ältere Version des NuGet-Anbieters auf Ihrem Computer installiert ist, wird mitunter die ältere NuGet-Version zuerst in die PowerShell-Sitzung geladen (was die Racebedingung in OneGet ist). Damit PowerShellGet funktioniert, ist jedoch die neuere Version des NuGet-Anbieters erforderlich. Deshalb wird OneGet von PowerShellGet aufgefordert, für den NuGet-Anbieter erneut das Bootstrapping auszuführen. Dies führt zu mehreren Aufforderungen zum Bootstrapping des NuGet-Anbieters.

**Lösung**: In WMF 5.1 lädt OneGet nun die neueste Version des NuGet-Anbieters, um mehrere Aufforderungen zum Bootstrapping des NuGet-Anbieters zu vermeiden.

Es gibt auch eine Umgehung dieses Problems. Löschen Sie dazu manuell die alte Version des NuGet-Anbieters (NuGet-Anycpu.exe), sofern vorhanden, aus „$env:ProgramFiles\PackageManagement\ProviderAssemblies $env:LOCALAPPDATA\PackageManagement\ProviderAssemblies“.


##Unterstützung für OneGet auf Computern mit ausschließlichem Intranetzugriff

**Szenario**: In WMF 5.0 hat OneGet keine Computer unterstützt, die nur auf das Intranet (aber nicht auf das Internet) zugreifen dürfen.

**Lösung**: In WMF 5.1 können Sie diese Schritte ausführen, um Intranetcomputern das Verwenden von OneGet zu erlauben:

1. Laden Sie den NuGet-Anbieter auf einem anderen Computer mit Internetverbindung mit dem Befehl „Install-PackageProvider NuGet“ herunter.

2. Den NuGet-Anbieter finden Sie entweder unter „$env:ProgramFiles\PackageManagement\ProviderAssemblies\nuget“ oder „$env:LOCALAPPDATA\PackageManagement\ProviderAssemblies\nuget“. 

3. Kopieren Sie die Binärdateien in einen Ordner oder eine Netzwerkfreigabe, auf die der Intranetcomputer zugreifen kann, und installieren Sie den NuGet-Anbieter mit „Install-PackageProvider NuGet -Source <Path to folder>“.


##Verbesserungen bei der Ereignisprotokollierung

Wenn Sie Pakete installieren, ändern Sie den Status des Computers. In WMF 5.1 protokolliert OneGet jetzt beim Installieren, Deinstallieren und Speichern von Paketen Ereignisse im Windows-Ereignisprotokoll. Der Ereigniskanal ist identisch für PowerShell, d. h. Microsoft-Windows-PowerShell, Operational.

##Unterstützung für Standardauthentifizierung

In WMF 5.1 unterstützt OneGet das Suchen und Installieren von Paketen aus einem Repository, das Standardauthentifizierung erfordert. Sie können Ihre Anmeldeinformationen den Cmdlets „Find-Package“ und „Install-Package“ angeben. Beispiel:

``` PowerShell
Find-Package -Source <SourceWithCredential> -Credential (Get-Credential)
```
##Unterstützung der Verwendung von OneGet hinter einem Proxy

In WMF 5.1 verwendet OneGet die neuen Proxyparameter „-ProxyCredential“ und „-Proxy“. Mithilfe dieser Parameter können Sie die Proxy-URL und Anmeldeinformationen für OneGet-Cmdlets angeben. Standardmäßig werden die Proxyeinstellungen des Systems verwendet. Beispiel:

``` PowerShell
Find-Package -Source http://www.nuget.org/api/v2/ -Proxy http://www.myproxyserver.com -ProxyCredential (Get-Credential)
```



<!--HONumber=Oct16_HO1-->


