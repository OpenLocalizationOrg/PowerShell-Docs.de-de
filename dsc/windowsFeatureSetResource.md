---
title: "DSC-Ressource „WindowsFeatureSet“"
ms.date: 2016-05-24
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: df6869cdf1d1f6c823704e4de2882e90cb672ad2

---

# DSC-Ressource „WindowsFeatureSet“

> Gilt für: Windows PowerShell 4.0, Windows PowerShell 5.0

Die Ressource **WindowsFeatureSet** in Windows PowerShell DSC (Desired State Configuration) bietet einen Mechanismus, um sicherzustellen, dass Rollen und Features einem Zielknoten hinzugefügt oder von diesem entfernt werden.
Diese Ressource ist eine [zusammengesetzte Ressource](authoringResourceComposite.md), die die Ressource [WindowsFeature](windowsfeatureResource.md) für jedes Feature aufruft, das in der `Name`-Eigenschaft angegeben ist.

Verwenden Sie diese Ressource, wenn Sie verschiedene Windows-Features mit demselben Status konfigurieren möchten.

## Syntax

```
WindowsFeatureSet [string] #ResourceName
{
    Name = [string[]] 
    [ Ensure = [string] { Absent | Present }  ]
    [ Source = [string] ]
    [ IncludeAllSubFeature = [bool] ]
    [ Credential = [PSCredential] ]
    [ LogPath = [string] ]
    [ DependsOn = [string[]] ]
    
}
```

## Eigenschaften

|  Eigenschaft  |  Beschreibung   | 
|---|---| 
| Name| Die Namen der Rollen oder Features an, die hinzugefügt oder entfernt werden sollen. Dies ist identisch mit der **Name**-Eigenschaft des Cmdlets [Get-WindowsFeature](https://technet.microsoft.com/en-us/library/jj205469.aspx) und nicht mit dem Anzeigenamen der Rollen oder Features.| 
| Credential| Die Anmeldeinformationen zum Hinzufügen oder Entfernen der Rollen oder Features.| 
| Ensure| Gibt an, ob die Rollen oder Features hinzugefügt werden. Um sicherzustellen, dass die Rollen oder Features hinzugefügt werden, legen Sie diese Eigenschaft auf „Present“ fest. Um sicherzustellen, dass die Rollen oder Features entfernt werden, legen Sie diese Eigenschaft auf „Absent“ fest.| 
| IncludeAllSubFeature| Legen Sie diese Eigenschaft auf **$true** fest, um alle erforderlichen Teilfeatures in die Features einzubeziehen, die Sie mit der **Name**-Eigenschaft angeben.| 
| LogPath| Der Pfad zu einer Protokolldatei, in der der Ressourcenanbieter den Vorgang protokollieren soll.| 
| DependsOn| Gibt an, dass die Konfiguration einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird. Wenn beispielsweise die ID des Skriptblocks mit der Ressourcenkonfiguration, den Sie zuerst ausführen möchten, __ResourceName__ und dessen Typ __ResourceType__ ist, lautet die Syntax für das Verwenden dieser Eigenschaft `DependsOn = "[ResourceType]ResourceName"`.| 
| Source| Gibt bei Bedarf den Speicherort der Quelldatei an, die für die Installation verwendet werden soll.| 

## Beispiel

Mit der folgenden Konfiguration wird sichergestellt, dass die Features **Webserver** (IIS) und **SMTP-Server** sowie alle Teilfeatures installiert werden.

```powershell
configuration FeatureSetTest
{
    Import-DscResource -ModuleName PSDesiredStateConfiguration
    Node localhost
    {

        WindowsFeatureSet WindowsFeatureSetExample
        {
            Name                    = @("SMTP-Server", "Web-Server")
            Ensure                  = 'Present'
            IncludeAllSubFeature    = $true
        } 
    }
}
```




<!--HONumber=Oct16_HO1-->


