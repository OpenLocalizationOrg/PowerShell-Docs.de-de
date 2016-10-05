---
title: "DSC für Linux-Resource „nxPackage“"
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 31867cc7af96a3d8d527f5906d77bed5206940b4

---

# DSC für Linux-Resource „nxPackage“

Die Ressource **nxPackage** in PowerShell DSC bietet einen Mechanismus zum Verwalten von Paketen auf einem Linux-Knoten.

## Syntax

```
nxPackage <string> #ResourceName
{
    Name = <string>
    [ Ensure = <string> { Absent | Present }  ]
    [ PackageManager = <string> { Yum | Apt | Zypper } ]
    [ PackageGroup = <bool>]
    [ Arguments = <string> ]
    [ ReturnCode = <uint32> ]
    [ LogPath = <string> ]
    [ DependsOn = <string[]> ]
    
}
```

## Eigenschaften

|  Eigenschaft |  Beschreibung | 
|---|---|
| Name| Der Name des Pakets, für das Sie einen bestimmten Zustand sicherstellen möchten.| 
| Ensure| Bestimmt, ob das Vorhandensein des Pakets geprüft werden soll. Legen Sie diese Eigenschaft auf „Present“ fest, um sicherzustellen, dass das Paket vorhanden ist. Legen Sie sie auf „Absent“ fest, um sicherzustellen, dass das Paket nicht vorhanden ist. Der Standardwert ist „Present“.|  
| PackageManager| Unterstützte Werte sind „yum“, „apt“ und „zypper“. Gibt den Paket-Manager an, der zum Installieren von Paketen verwendet werden soll. Wenn **FilePath** angegeben ist, dient der angegebene Pfad zum Installieren des Pakets. Andernfalls wird ein Paket-Manager zum Installieren des Pakets aus einem vorkonfigurierten Repository verwendet. Wenn weder **PackageManager** noch **FilePath** angegeben ist, wird der standardmäßige Paket-Manager für das System verwendet.| 
| FilePath| Der Dateipfad, in dem das Paket gespeichert ist.| 
| PackageGroup| Falls **$true**, soll **Name** dem Namen einer Paketgruppe für die Verwendung mit einem **PackageManager** entsprechen. **PackageGroup** ist ungültig, wenn **FilePath** angegeben wird.| 
| Arguments| Eine Zeichenfolge mit Argumenten, die exakt wie angegeben an das Paket übergeben wird.| 
| ReturnCode| Der erwartete Rückgabecode. Wenn der tatsächliche Rückgabecode nicht dem erwarteten Wert entspricht, gibt die Konfiguration einen Fehler zurück.| 
| DependsOn | Gibt an, dass die Konfiguration einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird. Wenn beispielsweise die **ID** des Skriptblocks mit der Ressourcenkonfiguration, den Sie zuerst ausführen möchten, **ResourceName** und dessen Typ **ResourceType** ist, lautet die Syntax für das Verwenden dieser Eigenschaft `DependsOn = "[ResourceType]ResourceName"`.| 

## Beispiel

Im folgende Beispiel wird sichergestellt, dass das Paket mit dem Namen „httpd“ mit dem Paket-Manager „Yum“ auf einem Linux-Computer installiert wird.

```
Import-DSCResource -Module nx 

Node $node {
nxPackage httpd
{
    Name = "httpd"
    Ensure = "Present"
    PackageManager = "Yum"
}
}
```




<!--HONumber=Oct16_HO1-->


