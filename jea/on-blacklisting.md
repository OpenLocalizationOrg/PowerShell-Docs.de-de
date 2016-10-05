---
description: 
manager: dongill
ms.topic: article
author: jpjofre
ms.prod: powershell
keywords: powershell,cmdlet,jea
ms.date: 2016-06-22
title: Blacklists
ms.technology: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 8892e5e08a763fbc66d782bbc9252d1f3a7dcfcf

---

### Blacklists
Nachdem Sie erste Erfahrungen mit JEA gesammelt haben, fragen Sie sich vielleicht, ob es möglich ist, Befehle auf eine „Blacklist“ zu setzen.
Das ist eine verständliche Anforderung, derzeit jedoch aus folgenden Gründen für JEA nicht geplant:

1.  Wir haben JEA dafür konzipiert, Operatoren nur die Aktionen zu erlauben, die sie ausführen müssen.
Eine Blacklist ist das Gegenteil dieses Konzepts.

2.  Die Autoren der PowerShell-Befehle haben diese Befehle nicht mit Blick auf JEA geschrieben.
Bei einer Neuinstallation von Windows Server 2016 sind etwa 1520 Befehle sofort verfügbar.
Bei den Gefahrenmodellen für diese Befehle ist die Möglichkeit, dass ein Benutzer Befehle mit einem Konto mit höheren Berechtigungen ausführt, nicht berücksichtigt.
Einige Befehle lassen per Design Codeeinfügungen zu (z. B. Add-Type und Invoke-Command im PowerShell-Kernmodul).
JEA kann Sie warnen, wenn Sie bestimmte, uns bekannte Befehle verfügbar machen, wir haben jedoch nicht jeden einzelnen Befehl in Windows basierend auf dem neuen Gefahrenmodell neu bewertet.
Sie müssen die Funktionen der Befehle verstehen, die Sie über JEA verfügbar machen.  

3.  Darüber hinaus: Selbst wenn JEA alle Befehle mit möglichen Schwachstellen durch Codeeinfügung blockieren würde, gäbe es keine Garantie, dass ein böswilliger Benutzer nicht in der Lage wäre, eine auf der Blacklist stehende Aktion über einen anderen Befehl auszuführen.
Wenn Sie die Befehle, die Sie verfügbar machen, nicht genau kennen, können Sie nicht garantieren, dass eine bestimmte Aktion nicht möglich ist.
Die Verantwortung dafür, die Befehle zu verstehen, die Sie verfügbar machen, liegt vollständig bei Ihnen – unabhängig davon, ob eine Whitelist oder eine Blacklist verwendet wird.
Die Anzahl von Befehlen, die auf einer Blacklist aufgeführt werden müssten, wäre unüberschaubar. Daher wird JEA stattdessen mit Whitelists implementiert.




<!--HONumber=Oct16_HO1-->


