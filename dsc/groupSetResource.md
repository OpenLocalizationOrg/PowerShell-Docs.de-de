---
title: "DSC-Ressource „GroupSet“"
ms.date: 2016-05-16
keywords: PowerShell DSC, integrierte, Ressource
description: "Stellt einen Mechanismus zum Verwalten lokaler Gruppen auf dem Zielknoten zur Verfügung."
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 45fe96c43a834566d398820e3d94e3be923bb385

---

# DSC-Ressource „GroupSet“

> Gilt für: Windows PowerShell 5.0

Die Ressource **GroupSet** in Windows PowerShell DSC bietet einen Mechanismus zum Verwalten lokaler Gruppen auf dem Zielknoten. Diese Ressource ist eine [zusammengesetzte Ressource](authoringResourceComposite.md), die die Ressource [Group](groupResource.md) für jede Gruppe aufruft, die im `GroupName`-Parameter angegeben ist.

Verwenden Sie diese Ressource, wenn Sie dieselbe Liste von Mitgliedern mehreren Gruppen hinzufügen oder aus diesen entfernen möchten, mehr als eine Gruppe entfernen möchten oder mehr als eine Gruppe mit derselben Liste von Mitgliedern hinzufügen möchten.

##Syntax##
```
Group [string] #ResourceName
{
    GroupName = [string[]]
    [ Ensure = [string] { Absent | Present }  ]
    [ MembersToInclude = [string[]] ]
    [ MembersToExclude = [string[]] ]
    [ Credential = [PSCredential] ]
    [ DependsOn = [string[]] ]
}
```

## Eigenschaften

|  Eigenschaft  |  Beschreibung   | 
|---|---| 
| GroupName| Die Namen der Gruppen, für die Sie einen bestimmten Zustand sicherstellen möchten.| 
| MembersToExclude| Verwenden Sie diese Eigenschaft, um Mitglieder aus der vorhandenen Gruppenmitgliedschaft zu entfernen. Der Wert dieser Eigenschaft ist ein Zeichenfolgenarray im Format *Domäne*\\*Benutzername*. Wenn Sie diese Eigenschaft in einer Konfiguration festgelegt haben, verwenden Sie die Eigenschaft **Members** nicht. Andernfalls wird ein Fehler generiert.| 
| Credential| Die Anmeldeinformationen für den Zugriff auf Remoteressourcen. **Hinweis**: Dieses Konto muss die entsprechenden Active Directory-Berechtigungen aufweisen, um alle nicht lokalen Konten zur Gruppe hinzuzufügen. Andernfalls tritt ein Fehler auf.
| Ensure| Gibt an, ob die Gruppen vorhanden sind. Legen Sie diese Eigenschaft auf „Absent“ fest, um sicherzustellen, dass die Gruppen nicht vorhanden sind. Das Festlegen auf „Present“ (den Standardwert) stellt sicher, dass die Gruppen vorhanden sind.| 
| Mitglieder| Verwenden Sie diese Eigenschaft, um die aktuelle Gruppenmitgliedschaft durch die angegebenen Member zu ersetzen. Der Wert dieser Eigenschaft ist ein Zeichenfolgenarray im Format *Domäne*\\*Benutzername*. Wenn Sie diese Eigenschaft in einer Konfiguration festlegen, verwenden Sie weder die Eigenschaft **MembersToExclude** noch die Eigenschaft **MembersToInclude**. Andernfalls wird ein Fehler generiert.| 
| MembersToInclude| Verwenden Sie diese Eigenschaft, um Member zur vorhandenen Gruppenmitgliedschaft hinzuzufügen. Der Wert dieser Eigenschaft ist ein Zeichenfolgenarray im Format *Domäne*\\*Benutzername*. Wenn Sie diese Eigenschaft in einer Konfiguration festgelegt haben, verwenden Sie die Eigenschaft **Members** nicht. Andernfalls wird ein Fehler generiert.| 
| DependsOn | Gibt an, dass die Konfiguration einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird. Wenn beispielsweise die ID des Skriptblocks mit der Ressourcenkonfiguration, den Sie zuerst ausführen möchten, __ResourceName__ und dessen Typ __ResourceType__ ist, lautet die Syntax für das Verwenden dieser Eigenschaft „DependsOn = „[ResourceType]ResourceName“.| 

## Beispiel 1

Im folgenden Beispiel wird gezeigt, wie Sie sicherstellen, dass die beiden Gruppen „myGroup“ und „myOtherGroup“ vorhanden sind. 

```powershell
configuration GroupSetTest
{
    Import-DscResource -ModuleName PSDesiredStateConfiguration
    Node localhost
    {
        GroupSet GroupSetTest
        {
            GroupName        = @("myGroup", "myOtherGroup")
            Ensure           = "Present"
            MembersToInclude = @("contoso\alice", "contoso\bob")
            MembersToExclude = $("contoso\john")
            Credential       = Get-Credential
        }
    }
}
$cd = @{
    AllNodes = @(
        @{
            NodeName                    = 'localhost'
            PSDscAllowPlainTextPassword = $true
            PSDscAllowDomainUser        = $true
        }
    )
}


GroupSetTest -ConfigurationData $cd
```

>**Hinweis:** In diesem Beispiel werden der Einfachheit halber unverschlüsselte Anmeldeinformationen verwendet. Informationen zum Verschlüsseln von Anmeldeinformationen in der MOF-Konfigurationsdatei finden Sie unter [Schützen der MOF-Datei](secureMOF.md).





<!--HONumber=Oct16_HO1-->


