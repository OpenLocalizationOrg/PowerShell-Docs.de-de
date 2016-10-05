---
title: "DSC-Ressource „Registry“"
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 48b68a99baa489dad38e7072b171db10ee0f7386

---

# DSC-Ressource „Registry“

> Gilt für: Windows PowerShell 4.0, Windows PowerShell 5.0

Die Ressource **Registry** in Windows PowerShell DSC bietet einen Mechanismus zum Verwalten von Registrierungsschlüsseln und -werten auf einem Zielknoten.

## Syntax

```
Registry [string] #ResourceName
{
    Key = [string]
    ValueName = [string]
    [ Ensure = [string] { Absent | Present }  ]
    [ Force =  [bool]   ]
    [ Hex = [bool] ]
    [ DependsOn = [string[]] ]
    [ ValueData = [string[]] ]
    [ ValueType = [string] { Binary | Dword | ExpandString | MultiString | Qword | String }  ]
}
```

## Eigenschaften
|  Eigenschaft  |  Beschreibung   | 
|---|---| 
| Key| Gibt den Pfad des Registrierungsschlüssels an, für den Sie einen bestimmten Zustand sicherstellen möchten. Dieser Pfad muss die Struktur enthalten.| 
| ValueName| Gibt den Namen des Registrierungswerts an.| 
| Ensure| Gibt an, ob Schlüssel und Wert vorhanden sind. Um dies sicherzustellen, legen Sie diese Eigenschaft auf „Present“ fest. Um sicherzustellen, dass sie nicht vorhanden sind, legen Sie die Eigenschaft auf „Absent“ fest. Der Standardwert ist „Present“.| 
| Force| Wenn der angegebene Registrierungsschlüssel vorhanden ist, wird dieser von __Force__ mit dem neuen Wert überschrieben.| 
| Hex| Gibt an, ob Daten im Hexadezimalformat ausgedrückt werden. Falls angegeben, werden die DWORD/QWORD-Wertdaten im Hexadezimalformat angezeigt. Gilt nicht für andere Typen. Der Standardwert ist __$false__.| 
| DependsOn| Gibt an, dass die Konfiguration einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird. Wenn beispielsweise die ID des Skriptblocks mit der Ressourcenkonfiguration, den Sie zuerst ausführen möchten, __ResourceName__ und dessen Typ __ResourceType__ ist, lautet die Syntax für das Verwenden dieser Eigenschaft `DependsOn = "[ResourceType]ResourceName"`.| 
| ValueData| Die Daten des Registrierungswerts.| 
| ValueType| Gibt den Typ des Werts an. Die unterstützten Typen sind: 
<ul><li>Zeichenfolge (REG_SZ)</li>


<li>Binär (REG-BINARY)</li>


<li>Dword 32-Bit (REG_DWORD)</li>


<li>Qword 64-Bit (REG_QWORD)</li>


<li>Mehrteilige Zeichenfolge (REG_MULTI_SZ)</li>


<li>Erweiterbare Zeichenfolge (REG_EXPAND_SZ)</li></ul>

## Beispiel
In diesem Beispiel wird sichergestellt, dass ein Schlüssel mit dem Namen "ExampleKey" vorhanden ist, in der **HKEY\_LOCAL\_MACHINE** Struktur.
```powershell
Configuration RegistryTest
{
    Registry RegistryExample
    {
        Ensure      = "Present"  # You can also set Ensure to "Absent"
        Key         = "HKEY_LOCAL_MACHINE\SOFTWARE\ExampleKey"
        ValueName   = "TestValue"
        ValueData   = "TestData"
    }
}
```

>**Hinweis:** Ändern einer Einstellung in der Registrierung der **HKEY\_CURRENT\_USER** Hive erfordert, dass die Konfiguration mit den Anmeldeinformationen des Benutzers und nicht als System ausgeführt wird.
>Sie können die **PsDscRunAsCredential**-Eigenschaft verwenden, um die Benutzeranmeldeinformationen für die Konfiguration anzugeben. Ein Beispiel finden Sie unter [Ausführen von DSC mit Benutzeranmeldeinformationen](runAsUser.md).






<!--HONumber=Oct16_HO1-->


