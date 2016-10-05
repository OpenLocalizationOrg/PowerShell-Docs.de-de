---
title: "DSC-Ressource „WindowsFeature“"
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 522dbea958a60f76e98abe32e11e6491ea6d457c

---

# DSC-Ressource „WindowsFeature“

> Gilt für: Windows PowerShell 4.0, Windows PowerShell 5.0

Die Ressource **WindowsFeature** in Windows PowerShell DSC (Desired State Configuration) bietet einen Mechanismus, um sicherzustellen, dass Rollen und Features einem Knoten hinzugefügt oder entfernt werden.

## Syntax

```
WindowsFeature [string] #ResourceName
{
    Name = [string]
    [ Credential = [PSCredential] ]
    [ Ensure = [string] { Absent | Present }  ]
    [ IncludeAllSubFeature = [bool] ]
    [ LogPath = [string] ]
    [ DependsOn = [string[]] ]
    [ Source = [string] ]
}
```

## Eigenschaften

|  Eigenschaft  |  Beschreibung   | 
|---|---| 
| Name| Gibt den Namen der Rolle oder des Features an, die/das hinzugefügt oder entfernt werden soll. Dies ist identisch mit der __Name__-Eigenschaft aus dem Cmdlet [Get-WindowsFeature](https://technet.microsoft.com/en-us/library/jj205469.aspx) und nicht mit dem Anzeigenamen der Rolle oder des Features.| 
| Credential| Gibt die Anmeldeinformationen zum Hinzufügen oder Entfernen der Rolle oder des Features an.| 
| Ensure| Gibt an, ob die Rolle oder das Feature hinzugefügt wird. Um sicherzustellen, dass die Rolle oder das Feature hinzugefügt wird, legen Sie diese Eigenschaft auf „Present“ fest. Um sicherzustellen, dass die Rolle oder das Feature entfernt wird, legen Sie diese Eigenschaft auf „Absent“ fest.| 
| IncludeAllSubFeature| Legen Sie diese Eigenschaft auf __$true__ fest, um den Status aller erforderlichen Teilfeatures mit dem Status des Features sicherzustellen, das Sie mit der __Name__-Eigenschaft angeben.| 
| LogPath| Gibt den Pfad zu einer Protokolldatei an, in der der Ressourcenanbieter den Vorgang protokollieren soll.| 
| DependsOn| Gibt an, dass die Konfiguration einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird. Wenn beispielsweise die ID des Skriptblocks mit der Ressourcenkonfiguration, den Sie zuerst ausführen möchten, __ResourceName__ und dessen Typ __ResourceType__ ist, lautet die Syntax für das Verwenden dieser Eigenschaft `DependsOn = "[ResourceType]ResourceName"`.| 
| Source| Gibt bei Bedarf den Speicherort der Quelldatei an, die für die Installation verwendet werden soll.| 

## Beispiel
```powershell
WindowsFeature RoleExample
{
    Ensure = "Present" 
    # Alternatively, to ensure the role is uninstalled, set Ensure to "Absent"
    Name = "Web-Server" # Use the Name property from Get-WindowsFeature  
}
```




<!--HONumber=Oct16_HO1-->


