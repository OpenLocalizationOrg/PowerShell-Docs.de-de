---
title: Installieren und Konfigurieren von WMF 5.1 (Preview)
ms.date: 2016-05-16
keywords: PowerShell, DSC, WMF
description: 
ms.topic: article
contributor: kriscv
manager: dongill
ms.prod: powershell
ms.technology: WMF
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 30445aecee478d4fceb6cd446f26f00fe2ecc3d8

---

# Installieren und Konfigurieren von WMF 5.1 (Preview) #

## Installieren von .NET 4.6
Um WMF 5.1 verwenden zu können, müssen Sie .NET Framework 4.6 installieren. Dieses Framework ist erforderlich, um die neuen Features für die Katalogsignatur zu aktivieren, die sich auf verschiedene Bereiche beim Laden von Modulen und Skripts in WMF 5.1 auswirken. 

[.NET Framework 4.6 ist in KB 3045560 verfügbar](https://support.microsoft.com/en-us/kb/3045560). Installationsanweisungen finden Sie auf der Downloadseite.

> **Hinweis:** Es ist ein bekanntes Problem, dass das WMF 5.1 Preview-Installationsprogramm nicht erkennt, dass .NET 4.6 eine erforderliche Komponente ist. Sie können WMF 5.1 Preview also vor der Installation von .NET 4.6 installieren. Unsere Tests haben gezeigt, dass Sie .NET 4.6 nach der Installation von WMF 5.1 Preview installieren können. In der finalen Version von WMF 5.1 wird vor der Installation überprüft, ob diese erforderliche Komponente vorhanden ist. 

## Herunterladen und Installieren von WMF 5.1 Preview

Laden Sie das WMF 5.1-Paket für das Betriebssystem und die Architektur herunter, in der es installiert werden soll:

| Betriebssystem       | Voraussetzungen | Paketlinks             |
|------------------------|---------------|---------------------------|
| Windows Server 2012 R2 | [.NET Framework 4.6](https://support.microsoft.com/en-us/kb/3045560) | [Win8.1AndW2K12R2-KB3156422-x64.msu](http://go.microsoft.com/fwlink/?LinkID=823586)|
| Windows Server 2012    | [.NET Framework 4.6](https://support.microsoft.com/en-us/kb/3045560) | [W2K12-KB3156423-x64.msu](http://go.microsoft.com/fwlink/?LinkID=823587)|
| Windows Server 2008 R2 | [.NET Framework 4.6](https://support.microsoft.com/en-us/kb/3045560) </br> [WMF 4.0](http://www.microsoft.com/en-us/download/details.aspx?id=40855) </br> Sicherheitsupdate für die [SHA-2-Codesignatur](https://technet.microsoft.com/en-us/library/security/3033929) | [Win7AndW2K8R2-KB3156424-x64.msu](http://go.microsoft.com/fwlink/?LinkID=823588) |
| Windows 8.1            | [.NET Framework 4.6](https://support.microsoft.com/en-us/kb/3045560) | **x64:** [Win8.1AndW2K12R2-KB3156422-x64.msu](http://go.microsoft.com/fwlink/?LinkID=823586) </br> **x86:** [Win8.1-KB3156422-x86.msu](http://go.microsoft.com/fwlink/?LinkID=823589) |
| Windows 7 SP1          | [.NET Framework 4.6](https://support.microsoft.com/en-us/kb/3045560) </br> [WMF 4.0](http://www.microsoft.com/en-us/download/details.aspx?id=40855) </br> Sicherheitsupdate für die [SHA-2-Codesignatur](https://technet.microsoft.com/en-us/library/security/3033929) | **x64:** [Win7AndW2K8R2-KB3156424-x64.msu](http://go.microsoft.com/fwlink/?LinkID=823588) </br> **x86:** [Win7-KB3156424-x86.msu](http://go.microsoft.com/fwlink/?LinkID=823590) |


## Installieren von WMF 5.1 über den Windows-Explorer (bzw. Datei-Explorer in Windows Server 2012 R2 und Windows 8.1)

1. Wechseln Sie zum Ordner, in den Sie die MSU-Datei heruntergeladen haben.

2. Doppelklicken Sie auf die MSU-Datei, um sie auszuführen.

## Installieren von WMF 5.1 über die Eingabeaufforderung##

1. Öffnen Sie nach dem Herunterladen des richtigen Pakets für die Architektur Ihres Computers ein Eingabeaufforderungsfenster mit erhöhten Benutzerrechten (Als Administrator ausführen). Bei den Server Core-Installationsoptionen von Windows Server 2012 R2, Windows Server 2012 oder Windows Server 2008 R2 SP1 wird das Eingabeaufforderungsfenster standardmäßig mit erhöhten Benutzerrechten geöffnet.

2. Wechseln Sie zum Ordner, in den Sie das Installationspaket für WMF 5.1 heruntergeladen oder kopiert haben.

3. Führen Sie einen der folgenden Befehle aus:
    - Führen Sie auf Computern mit Windows Server 2012 R2 oder Windows 8.1 x64 `Win8.1AndW2K12R2-KB3156422-x64.msu /quiet` aus.
    - Führen Sie auf Computern mit Windows Server 2012 `W2K12-KB3156423-x64.msu /quiet` aus.
    - Führen Sie auf Computern mit Windows Server 2008 R2 SP1 oder Windows 7 SP1 x64 `Win7AndW2K8R2-KB3156424-x64.msu /quiet` aus.
    - Führen Sie auf Computern mit Windows Server 8.1 x86 `Win8.1-KB3156422-x86.msu /quiet` aus.
    - Führen Sie auf Computern mit Windows Server 7 SP1 x86 `Win7-KB3156424-x86.msu /quiet` aus.

## Zusätzliche Installationshinweise für Windows Server 2008 SP1 und Windows 7 SP1##
Für Installation von WMF 5.1 unter Windows Server 2008 SP1 oder Windows 7 SP1 muss Folgendes installiert sein:
- Das neueste Service Pack
- [WMF 4.0](http://www.microsoft.com/en-us/download/details.aspx?id=40855)
- WMF 5.1 erfordert [Microsoft .NET Framework 4.6](https://support.microsoft.com/en-us/kb/3045560). Befolgen Sie die Anweisungen auf der Downloadseite, um Microsoft .NET Framework 4.6 zu installieren.
- Sicherheitsupdate für die [SHA-2-Codesignatur](https://technet.microsoft.com/en-us/library/security/3033929). Dies ist erforderlich, um die neuen PowerShell-Cmdlets für Windows-Katalogdateien zu verwenden. 

> **WinRM-Abhängigkeit:** Windows PowerShell DSC (Desired State Configuration) hängt von WinRM ab. Unter Windows Server 2008 R2 und Windows 7 ist WinRM nicht standardmäßig aktiviert. Führen Sie zum Aktivieren von WinRM in einer Windows PowerShell-Sitzung mit erhöhten Benutzerrechten `Set-WSManQuickConfig` aus.




<!--HONumber=Oct16_HO1-->


