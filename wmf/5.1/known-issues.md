---
title: Bekannte Probleme in WMF 5.1 (Preview)
ms.date: 2016-07-13
keywords: PowerShell, DSC, WMF
description: 
ms.topic: article
author: krishna
manager: dongill
ms.prod: powershell
ms.technology: WMF
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: d4c9e88ddd6cfaec611527d19d00cbd4db9f5d1d

---

#Bekannte Probleme in WMF 5.1 (Preview) #

> Hinweis: Diese Dokumentation ist vorläufig und kann geändert werden.

##Starten der PowerShell-Verknüpfung als Administrator
Nach der Installation von WMF erhalten Sie möglicherweise eine Fehlermeldung „Undefinierter Fehler“, wenn Sie versuchen, PowerShell als Administrator über die Verknüpfung zu starten.
Öffnen Sie die Verknüpfung erneut als Nicht-Administrator. Anschließend funktioniert die Verknüpfung auch, wenn Sie sie als Administrator verwenden.

##Pester
In diesem Release treten zwei Probleme auf, derer Sie sich bewusst sein sollten, wenn Sie Pester auf Nano Server verwenden:

* Wenn Pester selbst getestet wird, treten einige Fehler aufgrund von Unterschieden zwischen FullCLR und CoreCLR auf. Vor allem ist die Validate-Methode für den Typ „XmlDocument“ nicht verfügbar. Sechs Tests, die versuchen, das Schema der NUnit-Ausgabeprotokolle zu überprüfen, sind dafür bekannt, dass sie Fehler ausgeben. 
* Bei einem Code Coverage-Test tritt aktuell ein Fehler auf, da die DSC-Ressource *WindowsFeature* in Nano Server nicht vorhanden ist. Diese Fehler haben in der Regel jedoch keine Auswirkungen und können ohne Weiteres ignoriert werden.

##Überprüfung des Vorgangs 

* Bei „Update-Help“ tritt aufgrund eines nicht funktionierenden Hilfe-URIs ein Fehler für das Microsoft.PowerShell.Operation.Validation-Modul auf.



<!--HONumber=Oct16_HO1-->


