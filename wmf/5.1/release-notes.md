---
title: Anmerkungen zur WMF 5.1 Preview-Version
ms.date: 2016-07-27
keywords: PowerShell, DSC, WMF
description: 
ms.topic: article
author: jkeithb
manager: dongill
ms.prod: powershell
ms.technology: WMF
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 83061e651b190bab3e5914bb6270a5857f7aa7a5

---

# Windows Management Framework (WMF) 5.1 (Preview) – Anmerkungen zu dieser Version #

WMF 5.1 Preview umfasst PowerShell, WMI und WinRM sowie SIL-Komponenten (Softwareinventur und -lizenzierung), die mit Windows Server 2016 veröffentlicht werden. WMF 5.1 kann auf Windows 7, Windows 8.1, Windows Server 2008 R2, 2012 und 2012 R2 installiert werden und bietet gegenüber WMF 5.0 RTM eine Reihe von Vorteilen. Dazu zählen u. a.:

- Neue Cmdlets: lokale Benutzer und Gruppen; Get-ComputerInfo
- PowerShellGet-Verbesserungen wie z. B. die Durchsetzung von signierten Modulen und die Installation von JEA-Modulen
- Bei PackageManagement wurde Unterstützung für Container, CBS-Setup, EXE-basiertes Setup und CAB-Pakete hinzugefügt
- Debuggingverbesserungen für DSC und PowerShell-Klassen
- Sicherheitsverbesserungen wie u. a. die Durchsetzung von katalogsignierten Modulen vom Pullserver und bei Verwendung von PowerShellGet-Cmdlets
- Antworten auf verschiedene Benutzeranfragen und zu Problemen

**Wichtige Hinweise:**

- **Für WMF 5.1 Preview ist .NET Framework 4.6 erforderlich**. Die Installation ist erfolgreich, wichtige Features können jedoch nicht ausgeführt werden, wenn .NET 4.6 nicht installiert ist. Anweisungen finden Sie im Thema [Installieren und Konfigurieren von WMF 5.1 (Preview)](https://msdn.microsoft.com/en-us/powershell/wmf/5.1/install-configure). 
- **WMF 5.1 Preview wird für Produktionsbereitstellungen** aktuell nicht unterstützt. Diese Version wird bereitgestellt, damit Sie sich mit den verfügbaren Features vertraut machen und dem PowerShell-Team Feedback geben können.
- WMF 5.1 Preview kann direkt über WMF 5.0 installiert werden.
- Es ist ein bekanntes Problem, dass WMF 4.0 aktuell erforderlich ist, um WMF 5.1 Preview auf Windows 7 und Windows Server 2008 zu installieren. Diese Voraussetzung sollte für die finale Version nicht mehr gelten.
- Für die Installation zukünftiger Versionen von WMF 5.1, einschließlich der RTM-Version, muss WMF 5.1 Preview deinstalliert werden.




<!--HONumber=Oct16_HO1-->


