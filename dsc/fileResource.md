---
title: "DSC-Ressource „File“"
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 8c8fb7a40c066b048e1a54a741f4953e6b5a47b6

---

# DSC-Ressource „File“

> Gilt für: Windows PowerShell 4.0, Windows PowerShell 5.0

Die Ressource „File“ in Windows PowerShell DSC bietet einen Mechanismus zum Verwalten von Dateien und Verzeichnissen auf einem Zielknoten.

>**Hinweis:** Wenn die Eigenschaft **MatchSource** auf **$false** festgelegt wurde (der Standardwert), werden die zu kopierenden Inhalte bei der ersten Anwendung der Konfiguration zwischengespeichert. 
>Darauffolgende Anwendungen der Konfiguration suchen in dem in **SourcePath** angegebenen Pfad nicht nach aktualisierten Dateien bzw. Ordnern. Wenn Sie bei jeder Anwendung der Konfiguration nach Updates für die Dateien bzw. Ordner in **SourcePath** suchen möchten, müssen Sie **MatchSource** auf **$true** festlegen. 

## Syntax
```
File [string] #ResourceName
{
    DestinationPath = [string]
    [ Attributes = [string[]] { Archive | Hidden | ReadOnly | System }]
    [ Checksum = [string] { CreatedDate | ModifiedDate | SHA-1 | SHA-256 | SHA-512 } ]
    [ Contents = [string] ]
    [ Credential = [PSCredential] ]
    [ Ensure = [string] { Absent | Present } ] 
    [ Force = [bool] ]
    [ Recurse = [bool] ]
    [ DependsOn = [string[]] ]
    [ SourcePath = [string] ]
    [ Type = [string] { Directory | File } ] 
    [ MatchSource = [bool] ]
}
```

## Eigenschaften

|  Eigenschaft  |  Beschreibung   | 
|---|---| 
| DestinationPath| Gibt den Speicherort an, an dem Sie den Zustand einer Datei oder eines Verzeichnisses sicherstellen möchten.| 
| Attributes| Gibt den gewünschten Status der Attribute der Zieldatei oder des Zielverzeichnisses an.| 
| Checksum| Gibt den zu verwendenden Typ von Prüfsumme an, wenn bestimmt wird, ob zwei Dateien identisch sind. Wenn __Checksum__ nicht angegeben ist, wird nur der Datei- oder Verzeichnisname für den Vergleich verwendet. Gültige Werte sind: SHA-1, SHA-256, SHA-512, createdDate, modifiedDate.| 
| Contents| Gibt den Inhalt einer Datei an, z. B. eine bestimmte Zeichenfolge.| 
| Credential| Gibt die Anmeldeinformationen an, die für den Zugriff auf Ressourcen erforderlich sind, wie z. B. Quelldateien, wenn ein solcher Zugriff erforderlich ist.| 
| Ensure| Gibt an, ob die Datei oder das Verzeichnis vorhanden ist. Legen Sie diese Eigenschaft auf „Absent“ fest, um sicherzustellen, dass die Datei oder das Verzeichnis nicht vorhanden ist. Legen Sie sie auf „Present“ fest, um sicherzustellen, dass die Datei oder das Verzeichnis vorhanden ist. Die Standardeinstellung ist „Present“.| 
| Force| Bestimmte Dateioperationen (z. B. das Überschreiben einer Datei oder Löschen eines Verzeichnisses, das nicht leer ist), führen zu einem Fehler. Bei Verwenden der „Force“-Eigenschaft werden solche Fehler überschrieben. Der Standardwert ist __$false__.| 
| Recurse| Gibt an, ob Unterverzeichnisse enthalten sind. Legen Sie diese Eigenschaft auf __$true__ fest, um anzugeben, dass Unterverzeichnisse enthalten sein sollen. Der Standardwert ist __$false__. **Hinweis:** Diese Eigenschaft ist nur gültig, wenn die „Type“-Eigenschaft auf „directory“ festgelegt ist.| 
| DependsOn | Gibt an, dass die Konfiguration einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird. Wenn beispielsweise die ID des Skriptblocks mit der Ressourcenkonfiguration, den Sie zuerst ausführen möchten, __ResourceName__ und dessen Typ __ResourceType__ ist, lautet die Syntax für das Verwenden dieser Eigenschaft `DependsOn = "[ResourceType]ResourceName"`.| 
| SourcePath| Gibt den Pfad an, aus dem die Datei- oder Ordnerressource kopiert werden soll.| 
| Typ| Gibt an, ob die zu konfigurierende Ressource ein Verzeichnis oder eine Datei ist. Legen Sie diese Eigenschaft auf „Directory“ fest, um anzugeben, dass die Ressource ein Verzeichnis ist. Legen Sie sie auf „File“ fest, um anzugeben, dass die Ressource eine Datei ist. Der Standardwert ist „File“.| 
| MatchSource| Bei Festlegen auf den Standardwert __$false__ werden beliebige Dateien in der Quelle (z. B. die Dateien A, B und C) dem Ziel hinzugefügt, wenn die Konfiguration zum ersten Mal angewendet wird. Wenn eine neue Datei (D) der Quelle hinzugefügt wird, wird sie nicht dem Ziel hinzugefügt, auch wenn die Konfiguration später erneut angewendet wird. Wenn der Wert __$true__ ist, werden bei jedem Anwenden der Konfiguration in der Quelle gefundene neue Dateien (wie z. B. Datei D in diesem Beispiel) dem Ziel hinzugefügt. Der Standardwert ist **$false**.| 

## Beispiel

Im folgenden Beispiel wird veranschaulicht, wie die Ressource „File“ verwendet wird, um sicherzustellen, dass ein Verzeichnis mit dem Pfad `C:\Users\Public\Documents\DSCDemo\DemoSource` auf einem Quellcomputer (z. B. dem Pullserver) auch auf dem Zielknoten (mit allen Unterverzeichnissen) vorhanden ist. Außerdem wird nach Abschluss eine Bestätigungsmeldung in das Protokoll geschrieben und eine Anweisung hinzugefügt, um dafür zu sorgen, dass der Dateiprüfvorgang vor dem Protokollierungsvorgang erfolgt.

```powershell
Configuration FileResourceDemo
{
    Node "localhost"
    {
        File DirectoryCopy
        {
            Ensure = "Present"  # You can also set Ensure to "Absent"
            Type = "Directory" # Default is "File".
            Recurse = $true # Ensure presence of subdirectories, too
            SourcePath = "C:\Users\Public\Documents\DSCDemo\DemoSource"
            DestinationPath = "C:\Users\Public\Documents\DSCDemo\DemoDestination"    
        }

        Log AfterDirectoryCopy
        {
            # The message below gets written to the Microsoft-Windows-Desired State Configuration/Analytic log
            Message = "Finished running the file resource with ID DirectoryCopy"
            DependsOn = "[File]DirectoryCopy" # This means run "DirectoryCopy" first.
        }
    }
}
```




<!--HONumber=Oct16_HO1-->


