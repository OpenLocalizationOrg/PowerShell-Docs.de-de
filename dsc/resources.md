---
title: DSC-Ressourcen
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 0a27a40b995393c41f0496a5f7fa3f56fbd865dd

---

# DSC-Ressourcen

>Gilt für: Windows PowerShell 4.0, Windows PowerShell 5.0

DSC-Ressourcen sind die Bausteine einer DSC-Konfiguration. Eine Ressource macht Eigenschaften verfügbar, die konfiguriert werden können (Schema), und enthält die PowerShell-Skriptfunktionen, die der lokale Konfigurations-Manager wie gewünscht ausführt.

Mit einer Ressource kann etwas Allgemeines wie eine Datei oder Spezifisches wie eine IIS-Servereinstellung modelliert werden.  Gruppen ähnlicher Ressourcen werden in einem DSC-Modul kombiniert, das alle erforderlichen Dateien in einer Struktur organisiert, die portierbar ist und Metadaten zum Bestimmen des Zwecks der Ressourcen enthält.  

In den folgenden Themen werden DSC-Ressourcen beschrieben:

- [Integrierte DSC-Ressourcen](builtInResource.md)
- [Erstellen benutzerdefinierter DSC-Ressourcen](authoringResource.md)
- [Integrierte DSC-Ressourcen für Linux](lnxBuiltInResources.md)




<!--HONumber=Oct16_HO1-->


