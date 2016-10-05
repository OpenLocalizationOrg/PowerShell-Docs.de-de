---
title: "DSC für Linux-Resource „nxFile“"
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 2ba44df5dd6c91371cbbfe95d48184a4ff4a7738

---

# DSC für Linux-Resource „nxFile“

Die Ressource **nxFile** in PowerShell DSC bietet einen Mechanismus zum Verwalten von Dateien und Verzeichnissen auf einem Linux-Knoten.

## Syntax

```
nxFile <string> #ResourceName
{
    DestinationPath = <string>
    [ SourcePath = <string> ]
    [ Ensure = <string> { Absent | Present }  ]
    [ Type = <string> { directory | file | link } ]
    [ Contents = <string> ]
    [ Checksum = <string> { ctime | mtime | md5 }  ]
    [ Recurse = <bool> ]
    [ Force = <bool> ]
    [ Links = <string> { follow | manage } ]
    [ Group = <string> ]
    [ Mode = <string> ]
    [ Owner = <string> ]
    [ DependsOn = <string[]> ]

}
```

## Eigenschaften

|  Eigenschaft |  Beschreibung | 
|---|---|
| DestinationPath| Gibt den Speicherort an, an dem Sie den Zustand einer Datei oder eines Verzeichnisses sicherstellen möchten.| 
| SourcePath| Gibt den Pfad an, aus dem die Datei- oder Ordnerressource kopiert werden soll. Dieser Pfad kann ein lokaler Pfad oder eine URL des Typs `http/https/ftp` sein. Remote-URLs des Typs `http/https/ftp` werden nur unterstützt, wenn der Wert der **Type**-Eigenschaft „file“ ist.| 
| Ensure| Bestimmt, ob das Vorhandensein der Datei geprüft werden soll. Legen Sie diese Eigenschaft auf „Present“ fest, um sicherzustellen, dass die Datei vorhanden ist. Legen Sie sie auf „Absent“ fest, um sicherzustellen, dass die Datei nicht vorhanden ist. Der Standardwert ist „Present“.| 
| Type| Gibt an, ob die zu konfigurierende Ressource ein Verzeichnis oder eine Datei ist. Legen Sie diese Eigenschaft auf „Directory“ fest, um anzugeben, dass die Ressource ein Verzeichnis ist. Legen Sie sie auf „file“ fest, um anzugeben, dass die Ressource eine Datei ist. Der Standardwert ist „file“.| 
| Contents| Gibt den Inhalt einer Datei an, z. B. eine bestimmte Zeichenfolge.| 
| Checksum| Definiert den zu verwendenden Typ, wenn bestimmt wird, ob zwei Dateien identisch sind. Wenn **Checksum** nicht angegeben ist, wird nur der Datei- oder Verzeichnisname für den Vergleich verwendet. Werte sind: „ctime“, „mtime“ oder „md5“.| 
| Recurse| Gibt an, ob Unterverzeichnisse enthalten sind. Legen Sie diese Eigenschaft auf **$true** fest, um anzugeben, dass Unterverzeichnisse enthalten sein sollen. Der Standardwert ist **$false**. **Hinweis:** Diese Eigenschaft ist nur gültig, wenn die **Type**-Eigenschaft auf „directory“ festgelegt ist.| 
| Force| Bestimmte Dateioperationen (z. B. das Überschreiben einer Datei oder Löschen eines Verzeichnisses, das nicht leer ist), führen zu einem Fehler. Bei Verwenden der **Force**-Eigenschaft werden solche Fehler überschrieben. Der Standardwert ist **$false**.| 
| Links| Gibt das gewünschte Verhalten für symbolische Verknüpfungen an. Legen Sie diese Eigenschaft auf „follow“ fest, um symbolischen Verknüpfungen zu folgen und Aktionen auf das Ziel der Verknüpfung anzuwenden (z. B. die Datei statt der Verknüpfung kopieren). Legen Sie diese Eigenschaft auf „manage“ fest, um eine Aktion auf die Verknüpfung anzuwenden (z. B. die Verknüpfung selbst kopieren). Legen Sie diese Eigenschaft auf „ignore“ fest, um symbolische Verknüpfungen zu ignorieren.| 
| Group| Der Name der **Gruppe**, die die Datei oder das Verzeichnis besitzen soll.| 
| Mode| Gibt die gewünschten Berechtigungen für die Ressource in der Oktal- oder symbolischen Schreibweise an. (z. B. 777 oder rwxrwxrwx). Geben Sie bei Verwenden der symbolischen Schreibweise nicht das erste Zeichen an, welches Verzeichnis oder Datei angibt.| 
| DependsOn | Gibt an, dass die Konfiguration einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird. Wenn beispielsweise die **ID** des Skriptblocks mit der Ressourcenkonfiguration, den Sie zuerst ausführen möchten, **ResourceName** und dessen Typ **ResourceType** ist, lautet die Syntax für das Verwenden dieser Eigenschaft `DependsOn = "[ResourceType]ResourceName"`.| 

## Weitere Informationen


Linux und Windows verwenden in Textdateien standardmäßig unterschiedliche Zeilenumbruchzeichen. Diese kann zu unerwarteten Ergebnissen führen, wenn einige Dateien mit __nxFile__ auf einem Linux-Computer konfiguriert werden. Es gibt mehrere Möglichkeiten, den Inhalt einer Linux-Datei zu verwalten, um durch unerwartete Zeilenumbruchzeichen verursachte Probleme zu vermeiden:

Schritt 1: Kopieren Sie die Datei aus einer Remotequelle (HTTP, HTTPS oder FTP): Erstellen Sie eine Datei unter Linux mit dem gewünschten Inhalt, und stellen Sie sie auf einem Web- oder FTP-Server bereit, auf den die Knoten zugreifen können, die Sie konfigurieren. Legen Sie die __SourcePath__-Eigenschaft in der Ressource __nxFile__ auf die Web- oder FTP-URL zur Datei fest.

```
Import-DSCResource -Module nx
Node $Node
{
nxFile resolvConf
{
    SourcePath = "http://10.185.85.11/conf/resolv.conf"
    DestinationPath = "/etc/resolv.conf"
    Mode = "644"        
    Type = "file"
    
}
        
}
```


Schritt 2: Lesen Sie den Inhalt der Datei im PowerShell-Skript mit [Get-Content](https://technet.microsoft.com/en-us/library/hh849787.aspx), nachdem Sie die __$OFS__-Eigenschaft auf das Verwenden des Linux-Zeilenumbruchzeichens festgelegt haben.


```
Import-DSCResource -Module nx
Node $Node
{
$OFS = "`n"
$Contents = Get-Content C:\temp\resolv.conf

nxFile resolvConf
{
    DestinationPath = "/etc/resolv.conf"
    Mode = "644"        
    Type = "file"
    Contents = "$Contents"
}

}
```


Schritt 3: Verwenden Sie eine PowerShell-Funktion, um Windows-Zeilenumbrüche durch Linux-Zeilenumbruchzeichen zu ersetzen.

```
Function LinuxString($inputStr){
    $outputStr = $inputStr.Replace("`r`n","`n")
    $ouputStr += "`n"
    Return $outputStr
}

Import-DSCResource -Module nx
Node $Node
{

$Contents = @'
search contoso.com
domain contoso.com
nameserver 10.185.85.11
'@

$Contents = LinuxString $Contents

nxFile resolvConf
{
    DestinationPath = "/etc/resolv.conf"
    Mode = "644"        
    Type = "file"
    Contents = $Contents
    
}
}
```

## Beispiel

Im folgende Beispiel wird sichergestellt, dass das Verzeichnis `/opt/mydir` vorhanden ist und dass eine Datei mit dem angegebenen Inhalt in diesem Verzeichnis vorhanden ist.

```
Import-DSCResource -Module nx 

Node $node {
nxFile DirectoryExample
{
   Ensure = "Present"
   DestinationPath = "/opt/mydir"
   Type = "Directory"
}

nxFile FileExample
{
    Ensure = "Present"
    Destinationpath = "/opt/mydir/myfile"
    Contents=@"
#!/bin/bash`necho "hello world"`n
"@ 
    Mode = “755”
    DependsOn = "[nxFile]DirectoryExample"
} 
}
```




<!--HONumber=Oct16_HO1-->


