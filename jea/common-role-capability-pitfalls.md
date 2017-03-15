---
description: 
manager: dongill
ms.topic: article
author: jpjofre
ms.prod: powershell
keywords: powershell,cmdlet,jea
ms.date: 2016-06-22
title: "Häufige Fehler bei Rollenfunktionen"
ms.technology: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 2b2dfd9c39fe5d7bf4a52032653108729715e6bf

---

### Häufige Fehler bei Rollenfunktionen
Wenn Sie diesen Prozess alleine absolvieren, könnten Ihnen einige häufig auftretende Fehler unterlaufen.
Hier finden Sie einige wichtige Informationen dazu, wie Sie solche Fehler identifizieren und beheben, wenn Sie einen neuen Endpunkt erstellen oder ändern:

**Still link**: [これはテスト1です](これはテスト1です) > [This is a test2]( これはテスト2です ) 

**No Link**: [これはテスト3です]\(これはテスト3です\) > &#91;This is a test4&#93;(これはテスト4です)> [This is a test5]&#40;これはテスト5です&#41;



#### Funktionen im Vergleich zu Cmdlets
In PowerShell geschriebene PowerShell-Befehle sind PowerShell-Funktionen.
Als spezielle .NET-Klassen geschriebene PowerShell-Befehle sind PowerShell-Cmdlets.
Sie können den Befehlstyp prüfen, indem Sie `Get-Command` ausführen.

#### VisibleProviders
Sie müssen alle Anbieter verfügbar machen, die von Ihren Befehlen benötigt werden.
Am häufigsten wird der Dateisystemanbieter verwendet, Sie werden jedoch möglicherweise auch weitere Anbieter verfügbar machen müssen, beispielsweise den Registrierungsanbieter.
Eine Einführung in das Thema Anbieter finden Sie in diesem Blogbeitrag von [Hey, Scripting Guy!](http://blogs.technet.com/b/heyscriptingguy/archive/2015/04/20/find-and-use-windows-powershell-providers.aspx).
Gehen Sie vorsichtig vor, wenn Sie Anbieter verfügbar machen – häufig ist es besser, selbst eine Funktion zu definieren, die mit den zugrunde liegenden Anbietern arbeitet, als den Anbieter in einer JEA-Sitzung direkt verfügbar zu machen.
Auf diese Weise können Sie Benutzern weiterhin ermöglichen, mit Dateien, Registrierungsschlüsseln usw. zu arbeiten, behalten aber dank einer benutzerdefinierten Validierungslogik die Kontrolle darüber, mit **welchen** Dateien und Registrierungsschlüsseln die Benutzer arbeiten.




<!--HONumber=Oct16_HO1-->


