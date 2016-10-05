---
title: "DSC-Ressource „WindowsOptionalFeature“"
ms.date: 2016-05-24
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 1254037b655a3160c90e69971c9faf061f205917

---

# DSC-Ressource „WindowsOptionalFeature“

> Gilt für: Windows PowerShell 5.0

Die Ressource **WindowsOptionalFeature** in Windows PowerShell DSC (Desired State Configuration) bietet einen Mechanismus, um sicherzustellen, dass optionale Features auf einem Zielknoten aktiviert werden.

## Syntax

```
WindowsOptionalFeature [string] #ResourceName
{
    Name = [string]
    [ Ensure = [string] { Absent | Present }  ]
    [ Source = [string] ]
    [ NoWindowsUpdateCheck = [bool] ]
    [ RemoveFilesOnDisable = [bool] ]
    [ LogLevel = [string] { ErrorsOnly | ErrorsAndWarning | ErrorsAndWarningAndInformation }  ]
    [ LogPath = [string] ]
    [ DependsOn = [string[]] ]
    
}
```

## Eigenschaften

|  Eigenschaft  |  Beschreibung   | 
|---|---| 
| Name| Gibt den Namen des Features an, das aktiviert oder deaktiviert werden soll.| 
| Ensure| Gibt an, ob das Feature aktiviert ist. Um sicherzustellen, dass das Feature aktiviert ist, legen Sie diese Eigenschaft auf „Present“ fest. Um sicherzustellen, dass das Feature deaktiviert ist, legen Sie diese Eigenschaft auf „Absent“ fest.|
| Source| Nicht implementiert.|
| NoWindowsUpdateCheck| Gibt an, ob Windows Update (WU) von DISM kontaktiert wird, wenn die Quelldateien zum Aktivieren eines Features gesucht werden. Falls „$true“, wird WU nicht von DISM kontaktiert.|
| RemoveFilesOnDisable| Legen Sie diese Einstellung auf **$true** fest, um alle zum Feature gehörigen Dateien zu entfernen, wenn es deaktiviert wird (d. h. wenn **Ensure** auf „Absent“ festgelegt wird).|
| LogLevel| Die maximale Ausgabeebene, die in den Protokollen angezeigt wird. Die zulässigen Werte lauten: „ErrorsOnly“ (nur Fehler werden protokolliert), „ErrorsAndWarning“ (Fehler und Warnungen werden protokolliert) und „ErrorsAndWarningAndInformation“ (Fehler, Warnungen und Debuginformationen werden protokolliert).|
| LogPath| Der Pfad zu einer Protokolldatei, in der der Ressourcenanbieter den Vorgang protokollieren soll.| 
| DependsOn| Gibt an, dass die Konfiguration einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird. Wenn beispielsweise die ID des Skriptblocks mit der Ressourcenkonfiguration, den Sie zuerst ausführen möchten, __ResourceName__ und dessen Typ __ResourceType__ ist, lautet die Syntax für das Verwenden dieser Eigenschaft `DependsOn = "[ResourceType]ResourceName"`.| 
 






<!--HONumber=Oct16_HO1-->


