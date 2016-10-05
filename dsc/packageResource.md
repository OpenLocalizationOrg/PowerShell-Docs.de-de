---
title: "DSC-Ressource „Package“"
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: d07b11c148dfa747b3f9c0157191b83efb6c65df

---

# DSC-Ressource „Package“

> Gilt für: Windows PowerShell 4.0, Windows PowerShell 5.0

Die Ressource **Package** in Windows PowerShell DSC bietet einen Mechanismus zum Installieren oder Deinstallieren von Paketen, wie z. B. Windows Installer und „Setup.exe“-Pakete, auf einem Zielknoten.

## Syntax

```
Package [string] #ResourceName
{
    Name = [string]
    Path = [string]
    ProductId = [string]
    [ Arguments = [string] ]
    [ Credential = [PSCredential] ]
    [ Ensure = [string] { Absent | Present }  ]
    [ LogPath = [string] ]
    [ DependsOn = [string[]] ]
    [ ReturnCode = [UInt32[]] ]
}
```

## Eigenschaften
|  Eigenschaft  |  Beschreibung   | 
|---|---| 
| Name| Gibt den Namen des Pakets an, für das Sie einen bestimmten Zustand sicherstellen möchten.| 
| Path| Gibt den Pfad an, in dem das Paket gespeichert ist.| 
| ProductID| Gibt die Produkt-ID an, die das Paket eindeutig identifiziert.| 
| Arguments| Führt eine Zeichenfolge mit Argumenten auf, die exakt wie angegeben an das Paket übergeben wird.| 
| Credential| Ermöglicht den Zugriff auf das Paket für eine Remotequelle. Diese Eigenschaft wird nicht verwendet, um das Paket zu installieren. Das Paket wird immer auf dem lokalen System installiert.| 
| Ensure| Gibt an, ob das Paket installiert ist. Legen Sie diese Eigenschaft auf „Absent“ fest, um sicherzustellen, dass das Paket nicht installiert wird (oder deinstalliert wird, wenn es installiert ist). Legen Sie sie auf „Present“ (den Standardwert) fest, um sicherzustellen, dass das Paket installiert wird.| 
| LogPath| Gibt den vollständigen Pfad an, in dem der Anbieter eine Protokolldatei zum Installieren oder Deinstallieren des Pakets speichern soll.| 
| DependsOn | Gibt an, dass die Konfiguration einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird. Wenn beispielsweise die ID des Skriptblocks mit der Ressourcenkonfiguration, den Sie zuerst ausführen möchten, **ResourceName** und dessen Typ **ResourceType** ist, lautet die Syntax für das Verwenden dieser Eigenschaft „DependsOn = „[ResourceType]ResourceName“.| 
| ReturnCode| Gibt den erwarteten Rückgabecode an. Wenn der tatsächliche Rückgabecode nicht dem erwarteten Wert entspricht, gibt die Konfiguration einen Fehler zurück.| 

## Beispiel

Bei diesem Beispiel wird das MSI-Installationsprogramm ausgeführt, das sich im angegebenen Pfad befindet und die angegebene Produkt-ID hat.

```powershell
Configuration PackageTest
{
    Package PackageExample
    {
        Ensure      = "Present"  # You can also set Ensure to "Absent"
        Path        = "$Env:SystemDrive\TestFolder\TestProject.msi"
        Name        = "TestPackage"
        ProductId   = "ACDDCDAF-80C6-41E6-A1B9-8ABD8A05027E"
    } 
}
```




<!--HONumber=Oct16_HO1-->


