---
description: 
manager: dongill
ms.topic: article
author: jpjofre
ms.prod: powershell
keywords: powershell,cmdlet,jea
ms.date: 2016-06-22
title: "Überlegungen zum Einschränken von Befehlen"
ms.technology: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 9f3f79a29e0fb7ec5a5111284bb7985548e17749

---

### Überlegungen zum Einschränken von Befehlen
Ein wichtiger Hinweis zu diesem Schritt:
Es ist von entscheidender Bedeutung, dass alle über JEA verfügbar gemachten Funktionen sich in Bereichen befinden, auf die nur Administratoren zugreifen können.
Benutzer ohne Administratorrechte dürfen nicht die Möglichkeit haben, die verwendeten Skripts über JEA-Endpunkte zu ändern.

In diesem Zusammenhang ist es ebenso wichtig, dass Sie JEA-Benutzern nicht die Möglichkeit bieten, JEA-Konfigurationen und Skripts auf der Whitelist in ihren JEA-Sitzungen zu überschreiben.
Gehen Sie mit größter Umsicht vor, wenn Sie Befehle wie `Copy-Item` verfügbar machen!




<!--HONumber=Oct16_HO1-->


