---
title: Windows PowerShell-Systemanforderungen
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 6d1d3c75-3be4-4fc9-8805-ca9b2c454d42
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: b5b797ed09f9f43bfd0259e4af8b3754655d7c84

---

# Windows PowerShell-Systemanforderungen
In diesem Thema werden die Systemanforderungen für Windows PowerShell 3.0 und Windows PowerShell 4.0 sowie für besondere Features wie Windows PowerShell Integrated Scripting Environment (ISE), CIM-Befehle und Workflows aufgeführt.

Windows® 8.1 und Windows Server® 2012 R2 enthalten alle erforderlichen Programme. Dieses Thema ist für Benutzer früherer Versionen von Windows gedacht.

## Betriebssystemanforderungen
Windows PowerShell 4.0 wird unter den folgenden Versionen von Windows ausgeführt.

-   Windows 8.1, standardmäßig installiert

-   Windows Server 2012 R2, standardmäßig installiert

-   Windows® 7 mit Service Pack 1, [Windows Management Framework 4.0](http://go.microsoft.com/fwlink/?LinkId=293881) zum Ausführen von Windows PowerShell 4.0 installieren

-   Windows Server® 2008 R2 mit Service Pack 1, [Windows Management Framework 4.0](http://go.microsoft.com/fwlink/?LinkId=293881) zum Ausführen von Windows PowerShell 4.0 installieren

Windows PowerShell 3.0 wird unter den folgenden Versionen von Windows ausgeführt.

-   Windows 8, standardmäßig installiert

-   Windows Server 2012, standardmäßig installiert

-   Windows® 7 mit Service Pack 1, [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) zum Ausführen von Windows PowerShell 3.0 installieren

-   Windows Server® 2008 R2 mit Service Pack 1, [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) zum Ausführen von Windows PowerShell 3.0 installieren

-   Windows Server 2008 mit Service Pack 2, [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) zum Ausführen von Windows PowerShell 3.0 installieren

## Microsoft .NET Framework-Anforderungen
Windows PowerShell 4.0 erfordert die vollständige Installation von Microsoft .NET Framework 4.5. Windows 8.1 und Windows Server 2012 R2 enthalten standardmäßig Microsoft .NET Framework 4.5.

Windows PowerShell 3.0 erfordert die vollständige Installation von Microsoft .NET Framework 4. In Windows 8 und Windows Server 2012 ist Microsoft .NET Framework 4.5 standardmäßig enthalten, wodurch diese Anforderung erfüllt ist.

Informationen zum Installieren von Microsoft .NET Framework 4.5 (dotNetFx45_Full_setup.exe) finden Sie im Microsoft Download Center unter [Microsoft .NET Framework 4.5](http://go.microsoft.com/fwlink/?LinkID=242919).

Informationen zur vollständigen Installation von Microsoft .NET Framework 4 (dotNetFx40_Full_setup.exe) finden Sie im Microsoft Download Center unter [Microsoft .NET Framework 4](http://go.microsoft.com/fwlink/?LinkID=212931).

## WS-Management 3.0
Windows PowerShell 3.0 und Windows PowerShell 4.0 erfordern WS-Management 3.0, das den WinRM-Dienst und das WSMan-Protokoll unterstützt. Dieses Programm ist in Windows 8.1, Windows Server 2012 R2, Windows 8, Windows Server 2012, Windows Management Framework 4.0 und Windows Management Framework 3.0 enthalten.

## Windows-Verwaltungsinstrumentation 3.0 (Windows Management Instrumentation, WMI)
Windows PowerShell 3.0 und Windows PowerShell 4.0 erfordern Windows Management Instrumentation 3.0 (Windows-Verwaltungsinstrumentation, WMI). Dieses Programm ist in Windows 8.1, Windows Server 2012 R2, Windows 8, Windows Server 2012, Windows Management Framework 4.0 und Windows Management Framework 3.0 enthalten. Wenn dieses Programm nicht auf dem Computer installiert ist, werden Features, die WMI erfordern, wie z. B. CIM-Befehle, nicht ausgeführt.

## Common Language Runtime 4.0
Windows PowerShell 3.0 und Windows PowerShell 4.0 werden für Common Language Runtime (CLR) 4.0 kompiliert.

## Anforderungen an die grafische Benutzeroberfläche
Windows PowerShell ist eine konsolenbasierte Anwendung, die keine grafische Benutzeroberfläche erfordert. Deshalb eignet sie sich gut für Computer ohne Bildschirm oder Monitor bzw. ohne Benutzeroberfläche, z. B. die Server Core-Installationsoptionen von Windows Server 2012 R2 oder Windows Server 2012.

Allerdings erfordern einige Elemente, wie z. B. die folgenden, eine grafische Benutzeroberfläche. Weitere Informationen finden Sie im Hilfethema des jeweiligen Elements.

-   Windows PowerShell Integrated Scripting Environment (ISE)

-   Cmdlets

    1.  [Out-GridView](https://technet.microsoft.com/en-us/library/70915a86-d753-464e-8349-cba02316154c)

    2.  [Show-Command](https://technet.microsoft.com/en-us/library/65bba50b-91a8-49d5-80a2-a30fc684ba41)

    3.  [Show-ControlPanelItem](https://technet.microsoft.com/en-us/library/0685d42c-37cc-498f-acf6-0ecfeb0cb162)

    4.  [Show-EventLog](https://technet.microsoft.com/en-us/library/a3b0f5ad-0438-42c7-915b-d1b4793a431c)

-   Parameter

    1.  **ShowWindow**-Parameter des Cmdlets [Get-Help](https://technet.microsoft.com/en-us/library/1f46eeb4-49d7-4bec-bb29-395d9b42f54a).

    2.  **ShowSecurityDescriptorUI**-Parameter der Cmdlets [Register-PSSessionConfiguration](https://technet.microsoft.com/en-us/library/e9152ae2-bd6d-4056-9bc7-dc1893aa29ea) und [Set-PSSessionConfiguration](https://technet.microsoft.com/en-us/library/b21fbad3-1759-4260-b206-dcb8431cd6ea).

## Anforderungen an das Windows PowerShell-Modul
Windows PowerShell 4.0 ist mit Windows PowerShell 3.0 und Windows PowerShell 2.0 abwärtskompatibel. Cmdlets, Anbieter, Snap-Ins, Module und Skripts, die für Windows PowerShell 2.0 und Windows PowerShell 3.0 geschrieben wurden, können unverändert in Windows PowerShell 4.0 ausgeführt werden.

Doch aufgrund einer Änderung der Richtlinie für die Laufzeitaktivierung in Microsoft .NET Framework 4 können Windows PowerShell-Hostprogramme, die für Windows PowerShell 2.0 geschrieben und mit Common Language Runtime (CLR) 2.0 kompiliert wurden, nicht unverändert in Windows PowerShell 3.0 ausgeführt werden, das mit CLR 4.0 kompiliert wurde.

Windows PowerShell 2.0 Engine erfordert mindestens Microsoft .NET Framework 2.0.50727. Diese Anforderung wird durch Microsoft .NET Framework 3.5 Service Pack 1 erfüllt. Diese Anforderung wird nicht durch Microsoft .NET Framework 4 und höhere Versionen von Microsoft .NET Framework erfüllt.

Informationen zum Hinzufügen oder Installieren von Windows PowerShell 2.0 Engine sowie zum Hinzufügen oder Installieren der erforderlichen Versionen von Microsoft .NET Framework finden Sie unter [Installieren des Windows PowerShell 2.0-Moduls](Installing-the-Windows-PowerShell-2.0-Engine.md). Informationen zum Starten von Windows PowerShell 2.0 Engine finden Sie unter [Starten des Windows PowerShell 2.0-Moduls](Starting-the-Windows-PowerShell-2.0-Engine.md).

## Windows Preinstallation Environment
Windows PowerShell 2.0, Windows PowerShell 3.0 und Windows PowerShell 4.0 werden in Windows Preinstallation Environment (Windows PE) ausgeführt. Die folgenden Cmdlets werden jedoch nicht unterstützt.

-   [Background Intelligent Transfer Service (BITS)-Cmdlets](http://go.microsoft.com/fwlink/?LinkId=257514)

-   [Get-EventLog](https://technet.microsoft.com/en-us/library/b4985b11-82bf-487d-928d-becd96fc0419)

-   [Get-WinEvent](https://technet.microsoft.com/en-us/library/5fe94870-ed6b-4ce2-9500-93846cc65c95)

-   [Save-Help](https://technet.microsoft.com/en-us/library/aed94f90-b73f-4e25-a25d-7c18d9f161fa)

-   [Update-Help](https://technet.microsoft.com/en-us/library/93e1d870-ace6-432b-8778-8920291d7545)

Darüber hinaus ist der **WinRM**-Dienst nicht in der Windows PE vorhanden.

## Weitere Informationen
[Erste Schritte mit WindowsPowerShell](../getting-started/Getting-Started-with-Windows-PowerShell.md)

[Installieren von WindowsPowerShell](Installing-Windows-PowerShell.md)

[Starten von WindowsPowerShell](https://technet.microsoft.com/en-us/library/8ec8c2d7-8e7c-4722-a3d2-498fe5739a8e)




<!--HONumber=Oct16_HO1-->


