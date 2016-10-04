---
title: DSC-Konfigurationen
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: d84bb35ada3588367436e6f5e3c6696b90c3661b

---

# DSC-Konfigurationen

>Gilt für: Windows PowerShell 4.0, Windows PowerShell 5.0

DSC-Konfigurationen sind PowerShell-Skripts, die eine besondere Art von Funktion definieren. Zum Definieren einer Konfiguration verwenden Sie das PowerShell-Schlüsselwort __Configuration__.

```powershell
Configuration MyDscConfiguration {

    Node "TEST-PC1" {
        WindowsFeature MyFeatureInstance {
            Ensure = "Present"
            Name =  "RSAT"
        }
        WindowsFeature My2ndFeatureInstance {
            Ensure = "Present"
            Name = "Bitlocker"
        }
    }
}
```

Speichern Sie das Skript als PS1-Datei.

## Konfigurationssyntax

Ein Konfigurationsskript besteht aus den folgenden Teilen:

- Dem **Configuration**-Block. Dies ist die äußerste Skriptblock. Sie definieren ihn mithilfe des Schlüsselworts **Configuration** und der Angabe eines Namens. In diesem Fall lautet der Name der Konfiguration „MyDscConfiguration“.
- Mindestens einem **Node**-Block. Diese Blöcke definieren die Knoten (Computer oder VMs), die Sie konfigurieren. Bei der obigen Konfiguration gibt es einen **Node**-Block für den Computer „TEST-PC1“.
- Mindestens einen Ressourcenblock. In diesen Blöcken werden die Eigenschaften der Ressourcen festlegt, die konfiguriert werden. In diesem Fall gibt es zwei Ressourcenblöcke, die beide die Ressource „WindowsFeature“ aufrufen.

Innerhalb eines **Configuration**-Blocks sind alle Aktionen möglich, die Sie normalerweise mit einer PowerShell-Funktion ausführen. Wenn Sie beispielsweise im vorherigen Beispiel den Namen des Zielcomputers nicht in der Konfiguration hart codieren möchten, können Sie einen Parameter für den Knotennamen hinzufügen:

```powershell
Configuration MyDscConfiguration {

    param(
        [string[]]$ComputerName="localhost"
    )
    Node $ComputerName {
        WindowsFeature MyFeatureInstance {
            Ensure = "Present"
            Name =  "RSAT"
        }
        WindowsFeature My2ndFeatureInstance {
            Ensure = "Present"
            Name = "Bitlocker"
        }
    }
}
```

Bei diesem Beispiel geben Sie den Namen des Knotens an, indem Sie ihn als $ComputerName-Parameter übergeben, wenn Sie [die Konfiguration kompilieren](# Compiling the configuration). Der Standardname ist „localhost“.

## Kompilieren der Konfiguration
Bevor Sie eine Konfiguration anwenden können, müssen Sie sie in einem MOF-Dokument kompilieren. Dazu rufen Sie die Konfiguration wie eine PowerShell-Funktion auf.
>__Hinweis:__ Damit eine Konfiguration aufgerufen werden kann, muss sie sich (wie jede andere PowerShell-Funktion) im globalen Bereich befinden. Dies erfolgt, indem Sie das Skript mit „Dot-Sourcing“ ausführen oder das Konfigurationsskript durch Drücken von F5 oder Klicken auf die Schaltfläche __Skript ausführen__ in der integrierte Skriptumgebung starten. Um das Skript mit „Dot-Sourcing“ ausführen, rufen Sie den Befehl `. .\myConfig.ps1` auf, wobei `myConfig.ps1` der Name der Skriptdatei mit Ihrer Konfiguration ist.

Beim Aufruf der Konfiguration erfolgt Folgendes:

- Alle Variablen werden aufgelöst. 
- Im aktuellen Verzeichnis wird ein Ordner mit dem Namen der Konfiguration erstellt.
- Eine Datei namens _NodeName.mof_ wird im neuen Verzeichnis erstellt, wobei _NodeName_ der Name des Zielknotens der Konfiguration ist. Wenn mehrere Knoten vorhanden sind, wird eine MOF-Datei für jeden Knoten erstellt.

>__Hinweis:__: Die MOF-Datei enthält alle Konfigurationsinformationen für den Zielknoten. Aus diesem Grund ist es wichtig, sie geschützt zu halten. Weitere Informationen finden Sie unter [Schützen der MOF-Datei](secureMOF.md).

Das Kompilieren der ersten oben gezeigten Konfiguration führt zur folgenden Ordnerstruktur:

```powershell
PS C:\users\default\Documents\DSC Configurations> . .\MyDscConfiguration.ps1
PS C:\users\default\Documents\DSC Configurations> MyDscConfiguration
    Directory: C:\users\default\Documents\DSC Configurations\MyDscConfiguration
Mode                LastWriteTime         Length Name                                                                                              
----                -------------         ------ ----                                                                                         
-a----       10/23/2015   4:32 PM           2842 TEST-PC1.mof
```  

Wenn die Konfiguration wie im zweiten Beispiel einen Parameter verwendet, muss dieser zur Kompilierungszeit angegeben werden. Das sieht dann so aus:

```powershell
PS C:\users\default\Documents\DSC Configurations> . .\MyDscConfiguration.ps1
PS C:\users\default\Documents\DSC Configurations> MyDscConfiguration -ComputerName 'MyTestNode'
    Directory: C:\users\default\Documents\DSC Configurations\MyDscConfiguration
Mode                LastWriteTime         Length Name                                                                                              
----                -------------         ------ ----                                                                                         
-a----       10/23/2015   4:32 PM           2842 MyTestNode.mof
```      

## Verwenden von „DependsOn“
Eine nützliches DSC-Schlüsselwort ist __DependsOn__. In der Regel (jedoch nicht unbedingt immer) wendet DSC die Ressourcen in der Reihenfolge an, die der sie in der Konfiguration angegeben sind. __DependsOn__ gibt hingegen an, welche Ressourcen von anderen Ressourcen abhängig sind. Der LCM stellt sicher, dass sie in der richtigen Reihenfolge angewendet werden, und zwar unabhängig von der Reihenfolge, in der Ressourceninstanzen definiert sind. Eine Konfiguration kann z. B. angeben, dass eine Instanz der Ressource __User__ vom Vorhandensein einer __Group__-Instanz abhängig ist:

```powershell
Configuration DependsOnExample {
    Node Test-PC1 {
        Group GroupExample {
            Ensure = "Present"
            GroupName = "TestGroup"
        }

        User UserExample {
            Ensure = "Present"
            UserName = "TestUser"
            FullName = "TestUser"
            DependsOn = "[Group]GroupExample"
        }
    }
}
```

## Verwenden neuer Ressourcen in Ihrer Konfiguration
Wenn Sie die vorherigen Beispielen ausgeführt haben, werden Sie vielleicht die Warnung bemerkt haben, dass Sie eine Ressource verwendet haben, ohne sie explizit zu importieren.
Derzeit gehören 12 Ressourcen zum Funktionsumfang von DSC im „PSDesiredStateConfiguration“-Modul. Andere Ressourcen in externen Modulen müssen in `$env:PSModulePath` eingefügt werden, damit sie vom lokalen Konfigurations-Manager (LCM) erkannt werden. Das neue Cmdlet [Get-DscResource](https://technet.microsoft.com/en-us/library/dn521625.aspx) kann verwendet werden, um zu bestimmen, welche Ressourcen im System installiert sind und dem LCM zur Verfügung stehen. Nachdem diese Module in `$env:PSModulePath` abgelegt und von [Get-DscResource](https://technet.microsoft.com/en-us/library/dn521625.aspx) ordnungsgemäß erkannt wurden, müssen sie dennoch in Ihre Konfiguration geladen werden. __Import-DscResource__ ist ein dynamisches Schlüsselwort, das nur in einem __Configuration__-Block erkannt werden kann (d. h. es ist kein Cmdlet). __Import-DscResource__ unterstützt zwei Parameter:
* __ModuleName__ ist die empfohlene Methode der Verwendung von __Import DscResource__. Dieser Parameter akzeptiert den Namen des Moduls mit den Ressourcen, die importiert werden sollen, (sowie einem Zeichenfolgenarray mit Modulnamen). 
* __Name__ ist der Name der zu importierenden Ressource. Dies ist nicht der Anzeigename, der als „Name“ von [Get-DscResource](https://technet.microsoft.com/en-us/library/dn521625.aspx) zurückgegeben wird, sondern der Klassennamen, der verwendet wird, wenn Sie das Ressourcenschema definieren (wird als __ResourceType__ von [Get-DscResource](https://technet.microsoft.com/en-us/library/dn521625.aspx) zurückgegeben). 

## Weitere Informationen
* [Windows PowerShell DSC – Übersicht](overview.md)
* [DSC-Ressourcen](resources.md)
* [Konfigurieren des lokalen Konfigurations-Managers](metaConfig.md)




<!--HONumber=Oct16_HO1-->


