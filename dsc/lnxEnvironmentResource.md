---
title: "DSC für Linux-Resource „nxEnvironment“"
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 0a7ab24ff278defd7fc0a80f1dbd45bfa0e16427

---

# DSC für Linux-Resource „nxEnvironment“

Die Ressource **nxEnvironment** in PowerShell DSC bietet einen Mechanismus zum Verwalten von Systemumgebungsvariablen auf einem Linux-Knoten.

## Syntax

```
nxEnvironment <string> #ResourceName
{
    Name = <string>
    [ Value = <string>
    [ Ensure = <string> { Absent | Present }  ]
    [ Path = <bool> }
    [ DependsOn = <string[]> ]

}
```

## Eigenschaften

|  Eigenschaft |  Beschreibung | 
|---|---|
| Name| Gibt den Namen der Umgebungsvariablen an, für die Sie einen bestimmten Zustand sicherstellen möchten.| 
| Value| Der Wert, der der Umgebungsvariablen zugewiesen werden soll.| 
| Ensure| Bestimmt, ob das Vorhandensein der Variablen geprüft werden soll. Legen Sie diese Eigenschaft auf „Present“ fest, um sicherzustellen, dass die Variable vorhanden ist. Legen Sie sie auf „Absent“ fest, um sicherzustellen, dass die Variable nicht vorhanden ist. Der Standardwert ist „Present“.| 
| Path| Definiert die Umgebungsvariable, die konfiguriert wird. Legen Sie diese Eigenschaft auf **$true** fest, wenn die Variable die **Path**-Variable ist. Legen Sie sie andernfalls auf **$false** fest. Der Standardwert ist **$false**. Wenn die konfigurierte Variable die **Path**-Variable ist, wird der von der **Value**-Eigenschaft bereitgestellte Wert an den vorhandenen Wert angefügt.| 
| DependsOn | Gibt an, dass die Konfiguration einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird. Wenn beispielsweise die **ID** des Skriptblocks mit der Ressourcenkonfiguration, den Sie zuerst ausführen möchten, **ResourceName** und dessen Typ **ResourceType** ist, lautet die Syntax für das Verwenden dieser Eigenschaft `DependsOn = "[ResourceType]ResourceName"`.| 

## Weitere Informationen

* Wenn **Path** fehlt oder auf **$false** festgelegt ist, werden Umgebungsvariablen in `/etc/environment` verwaltet. Ihre Programme oder Skripts erfordern möglicherweise das Konfigurieren des Abrufs der Datei `/etc/environment` für den Zugriff auf die verwalteten Umgebungsvariablen.
* Wenn **Path** auf **$true** festgelegt ist, wird die Umgebungsvariable in der Datei `/etc/profile.d/DSCenvironment.sh` verwaltet. Diese Datei wird erstellt, falls sie nicht vorhanden ist. Wenn **Ensure** auf „Absent“ und **Path** auf **$true** festgelegt ist, wird eine vorhandene Umgebungsvariable nur aus `/etc/profile.d/DSCenvironment.sh` und nicht aus anderen Dateien entfernt.

## Beispiel

Das folgende Beispiel veranschaulicht das Verwenden der Ressource **nxEnvironment** zum Sicherstellen, dass **TestEnvironmentVariable** vorhanden ist und den Wert „Test-Value“ hat. Wenn **TestEnvironmentVariable** nicht vorhanden ist, wird die Variable erstellt.

```
Import-DSCResource -Module nx 


nxEnvironment EnvironmentExample
{
    Ensure = “Present”
    Name = “TestEnvironmentVariable”
    Value = “TestValue”
}
```





<!--HONumber=Oct16_HO1-->


