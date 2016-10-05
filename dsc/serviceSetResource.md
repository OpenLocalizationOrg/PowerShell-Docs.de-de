---
title: "DSC-Ressource „ServiceSet“"
ms.date: 2016-05-23
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 871d697626a0376e8f1f27bdbbf16d8612a56a79

---

# DSC-Ressource „ServiceSet“

> Gilt für: Windows PowerShell 4.0, Windows PowerShell 5.0


Die Ressource **ServiceSet** in Windows PowerShell DSC bietet einen Mechanismus zum Verwalten von Diensten auf dem Zielknoten. Diese Ressource ist eine [zusammengesetzte Ressource](authoringResourceComposite.md), die die Ressource [Service](serviceResource.md) für jeden Dienst aufruft, der in der `Name`-Eigenschaft angegeben ist.

Verwenden Sie diese Ressource, wenn Sie verschiedene Dienste mit demselben Status konfigurieren möchten.

## Syntax

```
Service [string] #ResourceName
{
    Name = [string[]]
    [ StartupType = [string] { Automatic | Disabled | Manual }  ]
    [ BuiltInAccount = [string] { LocalService | LocalSystem | NetworkService }  ]
    [ State = [string] { Running | Stopped }  ]
    [ Ensure = [string] { Absent | Present }  ]
    [ Credential = [PSCredential] ]
    [ DependsOn = [string[]] ]
    
}
```

## Eigenschaften

|  Eigenschaft  |  Beschreibung   | 
|---|---| 
| Name| Gibt den Namen des Diensts an. Beachten Sie, dass sich dieser mitunter vom Anzeigenamen unterscheidet. Mit dem Cmdlet [Get-Service](https://technet.microsoft.com/en-us/library/hh849804.aspx) können Sie eine Liste der Dienste und ihren aktuellen Status abrufen.|
| StartupType| Gibt den Starttyp für den Dienst an. Die für diese Eigenschaft zulässigen Werte sind **Automatic**, **Disabled** und **Manual**.|  
| BuiltInAccount| Gibt das zu verwendende Anmeldekonto für den Dienst an. Die für diese Eigenschaft zulässigen Werte sind **LocalService**, **LocalSystem** und **NetworkService**.| 
| Status| Gibt den Status an, den Sie für den Dienst sicherstellen möchten: **Stopped** oder **Running**.| 
| Ensure| Gibt an, ob die Dienste auf dem System vorhanden sind. Legen Sie diese Eigenschaft auf **Absent** fest, um sicherzustellen, dass die Dienste nicht vorhanden sind. Das Festlegen auf **Present** (den Standardwert) stellt sicher, dass Zieldienste vorhanden sind.|
| Credential| Gibt die Anmeldeinformationen für das Konto an, unter dem die Dienstressource ausgeführt wird. Diese Eigenschaft und die **BuiltinAccount**-Eigenschaft können nicht zusammen verwendet werden.| 
| DependsOn| Gibt an, dass die Konfiguration einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird. Wenn beispielsweise die ID des Skriptblocks mit der Ressourcenkonfiguration, den Sie zuerst ausführen möchten, *ResourceName* und dessen Typ *ResourceType* ist, lautet die Syntax für das Verwenden dieser Eigenschaft `DependsOn = "[ResourceType]ResourceName"`.| 



## Beispiel

Die folgende Konfiguration startet die Dienste „Windows-Audio“ und „Remotedesktopdienste“.

```powershell
configuration ServiceSetTest
{
    Import-DscResource -ModuleName PSDesiredStateConfiguration
    Node localhost
    {

        ServiceSet ServiceSetExample
        {
            Name        = @("TermService", "Audiosrv")
            StartupType = "Manual"
            State       = "Running"
        } 
    }
}
```




<!--HONumber=Oct16_HO1-->


