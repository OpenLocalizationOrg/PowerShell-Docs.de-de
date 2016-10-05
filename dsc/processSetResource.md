---
title: "DSC-Ressource „ProcessSet“"
ms.date: 2016-05-23
keywords: PowerShell DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 012a0e5c4f2a1f60ecea869d588b9c54e0567ced

---

# DSC-Ressource „WindowsProcess“

> Gilt für: Windows PowerShell 5.0

Die Ressource **ProcessSet** in Windows PowerShell DSC bietet einen Mechanismus zum Konfigurieren von Prozessen auf einem Zielknoten. Diese Ressource ist eine [zusammengesetzte Ressource](authoringResourceComposite.md), die die Ressource [WindowsProcess](windowsProcessResource.md) für jede Gruppe aufruft, die im `GroupName`-Parameter angegeben ist.

## Syntax

```
WindowsProcess [string] #ResourceName
{
    Arguments = [string]
    Path = [string]
    [ Credential = [PSCredential] ]
    [ Ensure = [string] { Absent | Present }  ]
    [ StandardOutputPath = [string] ]
    [ StandardErrorPath = [string] ]
    [ StandardInputPath = [string] ]   
    [ WorkingDirectory = [string] ]
    [ DependsOn = [string[]] ]
}
```

## Eigenschaften
|  Eigenschaft  |  Beschreibung   | 
|---|---| 
| Arguments| Eine Zeichenfolge mit Argumenten, die wie vorhanden an den Prozess übergeben werden. Wenn Sie mehrere Argumente übergeben müssen, fügen Sie sie alle dieser Zeichenfolge hinzu.| 
| Path| Die Pfade zu den ausführbaren Prozessdateien. Wenn es sich um die Namen der ausführbaren Dateien (keine vollqualifizierten Pfade) handelt, durchsucht die Ressource „DSC“ die Umgebungsvariable **Path** (`$env:Path`), um die Dateien zu ermitteln. Wenn die Werte dieser Eigenschaft vollqualifizierte Pfade sind, verwendet DSC nicht die Umgebungsvariable **Path**, um die Dateien zu finden, und löst einen Fehler aus, wenn beliebige der Pfade nicht vorhanden sind. Relative Pfade sind nicht zulässig.| 
| Credential| Gibt die Anmeldeinformationen zum Starten des Prozesses an.| 
| Ensure| Gibt an, ob der Prozess vorhanden ist. Legen Sie diese Eigenschaft auf „Present“ fest, um sicherzustellen, dass der Prozess vorhanden ist. Legen Sie sie andernfalls auf „Absent“ fest. Die Standardeinstellung ist „Present“.| 
| StandardErrorPath| Der Pfad, in den der Prozess Standardfehler schreibt. Eine vorhandene Datei wird überschrieben.| 
| StandardInputPath| Der Datenstrom, aus dem der Prozess die Standardeingabe empfängt.| 
| StandardOutputPath| Der Pfad der Datei, in die der Prozess die Standardausgabe schreibt. Eine vorhandene Datei wird überschrieben.| 
| WorkingDirectory| Der Speicherort, der als das aktuelle Arbeitsverzeichnis für den Prozess verwendet wird.| 
| DependsOn | Gibt an, dass die Konfiguration einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird. Wenn beispielsweise die ID des Skriptblocks mit der Ressourcenkonfiguration, den Sie zuerst ausführen möchten, **ResourceName** und dessen Typ **_ResourceType** ist, lautet die Syntax für das Verwenden dieser Eigenschaft so: DependsOn = "[ResourceType]ResourceName".| 




<!--HONumber=Oct16_HO1-->


