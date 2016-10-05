---
title: "DSC für die Linux-Resource „nxUser“"
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 7813185313845b74e2a37dfa4ec6bb109f32f0eb

---

# DSC für die Linux-Resource „nxUser“

Die Ressource **nxUser** in PowerShell DSC bietet einen Mechanismus zum Verwalten von Dateien und Verzeichnissen auf einem Linux-Knoten.

## Syntax

```
nxUser <string> #ResourceName
{
    UserName = <string>
    [ Ensure = <string> { Absent | Present }  ]
    [ FullName = <string> ]
    [ Description = <string> ]
    [ Password = <string> ]
    [ Disabled = <bool> ]
    [ PasswordChangeRequired = <bool> ]
    [ HomeDirectory = <string> ]
    [ Mode = <string> ]
    [ GroupID = <string> ]
    [ DependsOn = <string[]> ]

}
```

## Eigenschaften

|  Eigenschaft |  Gibt den Kontonamen an, für den Sie einen bestimmten Zustand sicherstellen möchten. | 
|---|---|
| UserName| Gibt den Speicherort an, an dem Sie den Zustand einer Datei oder eines Verzeichnisses sicherstellen möchten.| 
| Ensure| Gibt an, ob das Konto vorhanden ist. Legen Sie diese Eigenschaft auf „Present“ fest, um sicherzustellen, dass das Konto vorhanden ist. Legen Sie sie auf „Absent“ fest, um sicherzustellen, dass das Konto nicht vorhanden ist.| 
| FullName| Eine Zeichenfolge, die den vollständigen Namen des Benutzerkontos enthält.| 
| Description| Die Beschreibung des Benutzerkontos.| 
| Password| Der Hash des Benutzerkennworts im entsprechenden Format für den Linux-Computer. Dies ist normalerweise ein nach dem Zufallsprinzip gewählter SHA-256- oder SHA-512-Hash. Unter Debian und Ubuntu Linux kann dieser Wert mit dem Befehl „mkpasswd“ generiert werden. Für andere Linux-Distributionen kann die „crypt“-Methode der Crypt-Bibliothek von Python zum Generieren des Hashes verwendet werden.| 
| Disabled| Gibt an, ob das Konto aktiviert ist. Legen Sie diese Eigenschaft auf **$true** fest, um sicherzustellen, dass dieses Konto deaktiviert ist. Legen Sie sie auf **$false** fest, um sicherzustellen, dass es aktiviert ist.| 
| PasswordChangeRequired| Gibt an, ob der Benutzer das Kennwort ändern kann. Legen Sie diese Eigenschaft auf **$true** fest, um sicherzustellen, dass der Benutzer das Kennwort nicht ändern kann. Legen Sie es auf **$false** fest, damit der Benutzer das Kennwort ändern kann. Der Standardwert ist **$false**. Diese Eigenschaft wird nur ausgewertet, wenn das Benutzerkonto zuvor nicht vorhanden war und erstellt wird.| 
| HomeDirectory| Das Basisverzeichnis des Benutzers.| 
| GroupID| Die primäre Gruppen-ID des Benutzers.| 
| DependsOn | Gibt an, dass die Konfiguration einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird. Wenn beispielsweise die ID des Skriptblocks mit der Ressourcenkonfiguration, den Sie zuerst ausführen möchten, „ResourceName“ und dessen Typ „ResourceType“ ist, lautet die Syntax für das Verwenden dieser Eigenschaft `DependsOn = "[ResourceType]ResourceName"`.| 

## Beispiel

Im folgenden Beispiel wird sichergestellt, dass der Benutzer „monuser“ vorhanden und Mitglied der Gruppe „DBusers“ ist.

```
Import-DSCResource -Module nx 

Node $node {
nxUser UserExample{
   UserName = "monuser"
   Description = "Monitoring user"
   Password  =    '$6$fZAne/Qc$MZejMrOxDK0ogv9SLiBP5J5qZFBvXLnDu8HY1Oy7ycX.Y3C7mGPUfeQy3A82ev3zIabhDQnj2ayeuGn02CqE/0'
   Ensure = "Present"
   HomeDirectory = "/home/monuser"
}
 
nxGroup GroupExample{
   GroupName = "DBusers"
   Ensure = "Present"
   MembersToInclude = "monuser"
   DependsOn = "[nxUser]UserExample"            
}
}
```




<!--HONumber=Oct16_HO1-->


