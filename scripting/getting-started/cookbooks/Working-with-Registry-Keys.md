---
title: "Arbeiten mit Registrierungsschlüsseln"
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 91bfaecd-8684-48b4-ad86-065dfe6dc90a
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 4809eb60ba1a5529343c2ab3c88493bf2c32389b

---

# Arbeiten mit Registrierungsschlüsseln
Da Registrierungsschlüssel Elemente auf Windows PowerShell-Laufwerken sind, gleicht deren Verwendung der Arbeit mit Dateien und Ordnern. Ein wichtiger Unterschied besteht darin, dass jedes Element auf einem registrierungsbasierten Windows PowerShell-Laufwerk ein Container ist, genau wie ein Ordner auf einem Dateisystemlaufwerk. Registrierungseinträge und deren zugehörige Werte sind jedoch Eigenschaften der Elemente, nicht unterschiedliche Elemente.

### Auflisten aller Unterschlüssel eines Registrierungsschlüssels
Mit **Get-ChildItem** können Sie alle Elemente anzeigen, die sich unmittelbar in einem Registrierungsschlüssel befinden. Fügen Sie den optionalen Parameter **Force** hinzu, um ausgeblendete oder Systemelemente anzuzeigen. Dieser Befehl zeigt z.B. die Elemente an, die sich direkt auf dem Windows PowerShell-Laufwerk HKCU: befinden, das der Registrierungsstruktur HKEY_CURRENT_USER entspricht:

```
PS> Get-ChildItem -Path hkcu:\

   Hive: Microsoft.PowerShell.Core\Registry::HKEY_CURRENT_USER

SKC  VC Name                           Property
---  -- ----                           --------
  2   0 AppEvents                      {}
  7  33 Console                        {ColorTable00, ColorTable01, ColorTab...
 25   1 Control Panel                  {Opened}
  0   5 Environment                    {APR_ICONV_PATH, INCLUDE, LIB, TEMP...}
  1   7 Identities                     {Last Username, Last User ...
  4   0 Keyboard Layout                {}
...
```

Dies sind die im Registrierungs-Editor (Regedit.exe) unter HKEY_CURRENT_USER angezeigten Schlüssel der obersten Ebene.

Sie können diesen Registrierungspfad auch anhand des Namens des Registrierungsanbieters, gefolgt von „**::**“ angeben. Der vollständige Name des Registrierungsanbieters lautet **Microsoft.PowerShell.Core\Registry**, aber er kann auf **Registry** verkürzt werden. Jeder der folgenden Befehle listet die Inhalte direkt unter HKCU auf:

```
Get-ChildItem -Path Registry::HKEY_CURRENT_USER
Get-ChildItem -Path Microsoft.PowerShell.Core\Registry::HKEY_CURRENT_USER
Get-ChildItem -Path Registry::HKCU
Get-ChildItem -Path Microsoft.PowerShell.Core\Registry::HKCU
Get-ChildItem HKCU:
```

Diese Befehle listen nur die Elemente auf, die sich unmittelbar unter HKCU: befinden, ähnlich dem „Cmd.exe“-Befehl **DIR** oder **ls** in einer UNIX-Shell. Um enthaltene Elemente anzuzeigen, müssen Sie den Parameter **Recurse** angeben. Um alle Registrierungsschlüssel in HKCU aufzulisten, verwenden Sie den folgenden Befehl (dieser Vorgang kann extrem lange dauern):

```
Get-ChildItem -Path hkcu:\ -Recurse
```

**Get-ChildItem** stellt mit den Parametern **Path**, **Filter**, **Include** und **Exclude** komplexe Filterfunktionen zur Verfügung, die allerdings nur auf Namen basieren. Zum Durchführen komplexer Filterung basierend auf anderen Eigenschaften verwenden Sie das Cmdlet **Where-Object**. Der folgende Befehl sucht alle Schlüssel in „HKCU:\Software“, die nicht mehr als einen Unterschlüssel und zudem genau vier Werte enthalten:

```
Get-ChildItem -Path HKCU:\Software -Recurse | Where-Object -FilterScript {($_.SubKeyCount -le 1) -and ($_.ValueCount -eq 4) }
```

### Kopieren von Schlüsseln
Das Kopieren erfolgt mit **Copy-Item**. Der folgende Befehl kopiert „HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion“ und alle zugehörigen Eigenschaften nach „HKCU:\“ und erstellt dabei einen neuen Schlüssel mit dem Namen „CurrentVersion“:

```
Copy-Item -Path 'HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion' -Destination hkcu:
```

Wenn Sie diesen neuen Schlüssel im Registrierungs-Editor oder über **Get-ChildItem** untersuchen, stellen Sie fest, dass am neuen Speicherort keine Kopien der enthaltenen Unterschlüssel vorhanden sind. Um den gesamten Inhalt eines Containers zu kopieren, müssen Sie den Parameter **Recurse** angeben. Um den vorherigen Kopierbefehl rekursiv zu machen, verwenden Sie diesen Befehl:

```
Copy-Item -Path 'HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion' -Destination hkcu: -Recurse
```

Sie können auch weiterhin andere bereits verfügbare Tools verwenden, um Dateisystemkopien auszuführen. Alle Tools zur Bearbeitung der Registrierung (z. B. „reg.exe“, „regini.exe“ und „regedit.exe“) sowie COM-Objekte, die die Bearbeitung der Registrierung unterstützen (z. B. „WScript.Shell“ und die WMI-Klasse „StdRegProv“) können in Windows PowerShell verwendet werden.

### Erstellen von Schlüsseln
Das Erstellen neuer Schlüssel in der Registrierung ist einfacher als das Erstellen eines neuen Elements in einem Dateisystem. Da alle Registrierungsschlüssel Container sind, müssen Sie keinen Elementtyp, sondern einfach einen expliziten Pfad angeben, z. B.:

```
New-Item -Path hkcu:\software_DeleteMe
```

Sie können auch einen anbieterbasierten Pfad verwenden, um einen Schlüssel anzugeben:

```
New-Item -Path Registry::HKCU_DeleteMe
```

### Löschen von Schlüsseln
Das Löschen von Elementen funktioniert im Wesentlichen für alle Anbieter gleich. Die folgenden Befehle entfernen Elemente automatisch ohne Benutzereingriff:

```
Remove-Item -Path hkcu:\Software_DeleteMe
Remove-Item -Path 'hkcu:\key with spaces in the name'
```

### Entfernen aller Schlüssel unter einem bestimmten Schlüssel
Mit **Remove-Item** können Sie enthaltene Elemente entfernen. Wenn ein Element weitere Elemente enthält, werden Sie jedoch aufgefordert, das Entfernen zu bestätigen. Wenn Sie z. B. versuchen, den erstellten Unterschlüssel „HKCU:\CurrentVersion“ zu löschen, wird Folgendes angezeigt:

```
Remove-Item -Path hkcu:\CurrentVersion

Confirm
The item at HKCU:\CurrentVersion\AdminDebug has children and the -recurse
parameter was not specified. If you continue, all children will be removed with
 the item. Are you sure you want to continue?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help
(default is "Y"):
```

Um enthaltene Elemente ohne Rückfrage zu löschen, geben Sie den Parameter **-Recurse** an:

```
Remove-Item -Path HKCU:\CurrentVersion -Recurse
```

Wenn Sie alle Elemente in „HKCU:\CurrentVersion“ löschen möchten, aber nicht „HKCU:\CurrentVersion“ selbst, könnten Sie stattdessen Folgenden Befehl verwenden:

```
Remove-Item -Path HKCU:\CurrentVersion\* -Recurse
```




<!--HONumber=Oct16_HO1-->


