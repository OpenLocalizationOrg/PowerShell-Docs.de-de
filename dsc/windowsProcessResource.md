---
title: "DSC-Ressource „WindowsProcess“"
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 0fe5e7d9679d44bb50c897badf8c6517b95049e2

---

# DSC-Ressource „WindowsProcess“

> Gilt für: Windows PowerShell 4.0, Windows PowerShell 5.0

Die Ressource **WindowsProcess** in Windows PowerShell DSC bietet einen Mechanismus zum Konfigurieren von Prozessen auf einem Zielknoten.

## Syntax

```
WindowsProcess [string] #ResourceName
{
    Arguments = [string]
    Path = [string]
    [ Credential = [PSCredential] ]
    [ Ensure = [string] { Absent | Present }  ]
    [ DependsOn = [string[]] ]
    [ StandardErrorPath = [string] ]
    [ StandardInputPath = [string] ]
    [ StandardOutputPath = [string] ]
    [ WorkingDirectory = [string] ]
}
```

## Eigenschaften
|  Eigenschaft  |  Beschreibung   | 
|---|---| 
| Argumente| Gibt eine Zeichenfolge von Argumenten an, die wie vorhanden an den Prozess übergeben wird. Wenn Sie mehrere Argumente übergeben müssen, fügen Sie sie alle dieser Zeichenfolge hinzu.| 
| Path| Der Pfad zur ausführbaren Prozessdatei. Wenn es sich um den Namen der ausführbaren Datei (keinen vollqualifizierten Pfad) handelt, durchsucht die Ressource „DSC“ die Umgebungsvariable **Path** (`$env:Path`), um die ausführbare Datei zu ermitteln. Wenn der Werte dieser Eigenschaft ein vollqualifizierter Pfad ist, verwendet DSC nicht die Umgebungsvariable **Path**, um die Dateien zu finden, und löst einen Fehler aus, wenn der Pfad nicht vorhanden ist. Relative Pfade sind nicht zulässig.| 
| Credential| Gibt die Anmeldeinformationen zum Starten des Prozesses an.| 
| Ensure| Gibt an, ob der Prozess vorhanden ist. Legen Sie diese Eigenschaft auf „Present“ fest, um sicherzustellen, dass der Prozess vorhanden ist. Legen Sie sie andernfalls auf „Absent“ fest. Die Standardeinstellung ist „Present“.| 
| DependsOn | Gibt an, dass die Konfiguration einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird. Wenn beispielsweise die ID des Skriptblocks mit der Ressourcenkonfiguration, den Sie zuerst ausführen möchten, __ResourceName__ und dessen Typ __ResourceType__ ist, lautet die Syntax für das Verwenden dieser Eigenschaft „DependsOn = „[ResourceType]ResourceName“.| 
| StandardErrorPath| Gibt den Verzeichnispfad an, in den der Standardfehler geschrieben wird. Eine vorhandene Datei wird überschrieben.| 
| StandardInputPath| Gibt den Standardpfad für die Eingabe an.| 
| StandardOutputPath| Gibt den Speicherort an, in den die Standardausgabe geschrieben wird. Eine vorhandene Datei wird überschrieben.| 
| WorkingDirectory| Gibt den Speicherort an, der als das aktuelle Arbeitsverzeichnis für den Prozess verwendet wird.| 




<!--HONumber=Oct16_HO1-->


