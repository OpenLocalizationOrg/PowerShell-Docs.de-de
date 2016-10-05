---
title: Direktes Verarbeiten von Elementen
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 8cbd4867-917d-41ea-9ff0-b8e765509735
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: b21af4711cc5a846517c3e286c9e90f858612ccb

---

# Direktes Verarbeiten von Elementen
Die Elemente, die Sie in Windows PowerShell-Laufwerken sehen, z. B. Dateien und Ordner auf den Dateisystemlaufwerken, sowie die Registrierungsschlüssel in den Windows PowerShell-Registrierungslaufwerken werden in Windows PowerShell als *Elemente* (Items) bezeichnet. Die Cmdlets zum Arbeiten mit diesen Elementen haben das Substantiv **Item** in ihren Namen.

Die Ausgabe des Befehls **Get-Command -Noun Item** zeigt, dass es neun Windows PowerShell-Item-Cmdlets gibt.

```
PS> Get-Command -Noun Item

CommandType     Name                            Definition
-----------     ----                            ----------
Cmdlet          Clear-Item                      Clear-Item [-Path] <String[]...
Cmdlet          Copy-Item                       Copy-Item [-Path] <String[]>...
Cmdlet          Get-Item                        Get-Item [-Path] <String[]> ...
Cmdlet          Invoke-Item                     Invoke-Item [-Path] <String[...
Cmdlet          Move-Item                       Move-Item [-Path] <String[]>...
Cmdlet          New-Item                        New-Item [-Path] <String[]> ...
Cmdlet          Remove-Item                     Remove-Item [-Path] <String[...
Cmdlet          Rename-Item                     Rename-Item [-Path] <String>...
Cmdlet          Set-Item                        Set-Item [-Path] <String[]> ...
```

### Erstellen von neuen Elementen (New-Item)
Um ein neues Element im Dateisystem zu erstellen, verwenden Sie das Cmdlet **New-Item**. Fügen Sie den **Path**-Parameter mit dem Pfad zum Element und den **ItemType**-Parameter mit dem Wert „file“ oder „directory“ ein.

Wenn Sie beispielsweise ein neues Verzeichnis namens „New.Directory“ im Verzeichnis „C:\Temp“ erstellen möchten, geben Sie Folgendes ein:

```
PS> New-Item -Path c:\temp\New.Directory -ItemType Directory

    Directory: Microsoft.Windows PowerShell.Core\FileSystem::C:\temp

Mode                LastWriteTime     Length Name
----                -------------     ------ ----
d----        2006-05-18  11:29 AM            New.Directory
```

Um eine Datei zu erstellen, ändern Sie den Wert des **ItemType**-Parameters in „file“. Möchten Sie z. B. eine Datei namens „file1.txt“ im Verzeichnis „New.Directory“ erstellen, geben Sie Folgendes ein:

```
PS> New-Item -Path C:\temp\New.Directory\file1.txt -ItemType file

    Directory: Microsoft.Windows PowerShell.Core\FileSystem::C:\temp\New.Directory

Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a---        2006-05-18  11:44 AM          0 file1
```

Auf gleiche Weise können Sie einen neuen Registrierungsschlüssel erstellen. Tatsächlich ist ein Registrierungsschlüssel einfacher zu erstellen, weil der einzige Elementtyp in der Windows-Registrierung ein Schlüssel ist. (Registrierungseinträge sind Element-*Eigenschaften*.) Wenn Sie beispielsweise einen Schlüssel namens „_Test“ im „CurrentVersion“-Unterschlüssel erstellen möchten, geben Sie Folgendes ein:

```
PS> New-Item -Path HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion_Test

   Hive: Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Micros
oft\Windows\CurrentVersion

SKC  VC Name                           Property
---  -- ----                           --------
  0   0 _Test                          {}
```

Wenn Sie einen Registrierungspfad eingeben, müssen Sie den Doppelpunkt (**:**) in die Windows PowerShell-Laufwerksnamen, „HKLM:“ und „HKCU:“, einfügen. Ohne den Doppelpunkt erkennt Windows PowerShell den Laufwerksnamen im Pfad nicht.

### Warum Registrierungswerte keine Elemente
Wenn Sie mit dem Cmdlet **Get-ChildItem** nach den Elemente in einem Registrierungsschlüssel suchen, werden weder die tatsächlichen Registrierungseinträge noch deren Werte angezeigt.

Beispielsweise der Registrierungsschlüssel **HKEY_LOCAL_MACHINE\\Software\\Microsoft\\Windows\\CurrentVersion\\Run** enthält in der Regel mehrere Registrierungseinträge, die Anwendung darstellen, die beim Systemstart ausgeführt.

Wenn Sie **Get-ChildItem** verwenden, um nach untergeordnete Elementen im Schlüssel zu suchen, wird nur der **OptionalComponents**-Unterschlüssel des Schlüssels angezeigt:

```
PS> Get-ChildItem HKLM:\Software\Microsoft\Windows\CurrentVersion\Run
   Hive: Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\Software\Micros
oft\Windows\CurrentVersion\Run
SKC  VC Name                           Property
---  -- ----                           --------
  3   0 OptionalComponents             {}
```

Zwar wäre es praktisch, wenn Registrierungseinträge wie Elemente behandelt werden könnten, es ist aber nicht möglich, einen Pfad zu einem Registrierungseintrag so anzugeben, dass seine Eindeutigkeit sicherstellt ist. In der Pfadnotation wird nicht zwischen dem Registrierungsunterschlüssel namens **Run** und dem **(Standard)**Registrierungseintrag im **Run**-Unterschlüssel unterschieden. Darüber hinaus könnten Sie, wenn Registrierungseinträge Elemente wären, die Pfadnotation nicht dazu verwenden, einen Registrierungseintrag namens **Windows\CurrentVersion\Run** von dem Unterschlüssel zu unterscheiden, der sich in diesem Pfad befindet, denn Registrierungseintragsnamen können den umgekehrten Schrägstrich (**\\**) enthalten.

### Umbenennen von vorhandenen Elementen (Rename-Item)
Um den Namen einer Datei oder eines Ordners zu ändern, verwenden Sie das Cmdlet **Rename-Item**. Der folgende Befehl ändert den Namen der Datei **file1.txt** in **fileOne.txt**.

```
PS> Rename-Item -Path C:\temp\New.Directory\file1.txt fileOne.txt
```

Das Cmdlet **Rename-Item** kann den Namen einer Datei oder eines Ordners ändern, kann aber kein Element verschieben. Der folgende Befehl schlägt fehl, weil er versucht, die Datei aus dem Verzeichnis „New.Directory“ in das Verzeichnis „Temp“ zu verschieben.

```
PS> Rename-Item -Path C:\temp\New.Directory\fileOne.txt c:\temp\fileOne.txt
Rename-Item : Cannot rename because the target specified is not a path.
At line:1 char:12
+ Rename-Item  <<<< -Path C:\temp\New.Directory\fileOne c:\temp\fileOne.txt
```

### Verschieben von Elementen (Move-Item)
Um eine Datei oder einen Ordner zu verschieben, verwenden Sie das Cmdlet **Move-Item**.

Der folgende Befehl verschiebt z. B. das Verzeichnis „New.Directory“ aus dem Verzeichnis „C:\temp“ in das Stammverzeichnis von Laufwerk C:. Wenn Sie überprüfen möchten, ob das Element verschoben wurde, fügen Sie den **PassThru**-Parameter des **Move-Item**-Cmdlets ein. Ohne **Passthru** zeigt das Cmdlet **Move-Item** keine Ergebnisse an.

```
PS> Move-Item -Path C:\temp\New.Directory -Destination C:\ -PassThru

    Directory: Microsoft.Windows PowerShell.Core\FileSystem::C:\

Mode                LastWriteTime     Length Name
----                -------------     ------ ----
d----        2006-05-18  12:14 PM            New.Directory
```

### Kopieren von Elementen (Copy-Item)
Wenn Sie mit den Kopiervorgänge in anderen Shells vertraut sind, finden Sie das Verhalten des Cmdlets **Copy-Item** in Windows PowerShell möglicherweise ungewöhnlich. Wenn Sie ein Element aus einem Speicherort in einen anderen kopieren, kopiert „Copy-Item“ den Inhalt des Elements nicht standardmäßig.

Angenommen, Sie kopieren das Verzeichnis **New.Directory** von Laufwerk C: in das Verzeichnis „C:\temp“, dann wird der Befehl erfolgreich ausgeführt, aber die Dateien im Verzeichnis „New.Directory“ werden nicht kopiert.

```
PS> Copy-Item -Path C:\New.Directory -Destination C:\temp
```

Wenn Sie den Inhalt von **C:\temp\New.Directory** anzeigen, stellen Sie fest, dass es keine Dateien enthält:

```
PS> Get-ChildItem -Path C:\temp\New.Directory
PS>
```

Warum kopiert das Cmdlet **Copy-Item** den Inhalt nicht in den neuen Speicherort?

Das Cmdlet **Copy-Item** wurde als generisches Cmdlet entworfen, d.h., es ist nicht nur zum Kopieren von Dateien und Ordnern vorgesehen. Außerdem könnte es sein, auch wenn Dateien und Ordner kopiert werden, dass Sie nur den Container und nicht die Elemente im Container kopieren möchten.

Wenn Sie den gesamten Inhalt eines Ordners kopieren möchten, fügen Sie den **Recurse**-Parameter des **Copy-Item**-Cmdlets in den Befehl ein. Wenn Sie das Verzeichnis bereits ohne Inhalt kopiert haben, fügen Sie den **Force**-Parameter ein, der es ermöglicht, dass der leere Ordner überschrieben wird.

```
PS> Copy-Item -Path C:\New.Directory -Destination C:\temp -Recurse -Force -Passthru
    Directory: Microsoft.Windows PowerShell.Core\FileSystem::C:\temp

Mode                LastWriteTime     Length Name
----                -------------     ------ ----
d----        2006-05-18   1:53 PM            New.Directory

    Directory: Microsoft.Windows PowerShell.Core\FileSystem::C:\temp\New.Directory

Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a---        2006-05-18  11:44 AM          0 file1
```

### Löschen von Elementen (Remove-Item)
Um Dateien und Ordner zu löschen, verwenden Sie das Cmdlet **Remove-Item**. Windows PowerShell-Cmdlets, etwa **Remove-Item**, die erhebliche, nicht rückgängig zu machende Änderungen vornehmen, fordern häufig zu einer Bestätigung auf, wenn Sie deren Befehle eingeben. Wenn Sie beispielsweise versuchen, den Ordner **New.Directory** zu entfernen, werden Sie aufgefordert, den Befehl zu bestätigen, da der Ordner Dateien enthält:

```
PS> Remove-Item C:\New.Directory

Confirm
The item at C:\temp\New.Directory has children and the -recurse parameter was not
specified. If you continue, all children will be removed with the item. Are you
 sure you want to continue?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help
(default is "Y"):
```

Da **Ja** die Standardantwort ist, drücken Sie die **EINGABETASTE**, um den Ordner und alle in ihm enthaltenen Dateien zu löschen. Um den Ordner ohne Bestätigung zu löschen, verwenden Sie den **-Recurse**-Parameter.

```
PS> Remove-Item C:\temp\New.Directory -Recurse
```

### Ausführen von Elementen (Invoke-Item)
Windows PowerShell verwendet das **Invoke-Item**-Cmdlet, um eine Standardaktion für eine Datei oder einen Ordner auszuführen. Diese Standardaktion ist durch den Standardanwendungshandler in der Registrierung festgelegt. Der Effekt ist derselbe, als würden Sie im Datei-Explorer auf das Element doppelklicken.

Nehmen Sie an, Sie führen den folgenden Befehl aus:

```
PS> Invoke-Item C:\WINDOWS
```

Ein Explorer-Fenster, das in C:\\Windows befindet, nur angezeigt, als ob Sie den Ordner C:\\Windows doppelgeklickt wurde.

Wenn Sie die Datei **Boot.ini** auf einem System vor Windows Vista aufrufen:

```
PS> Invoke-Item C:\boot.ini
```

Ist der INI-Dateityp mit Editor verknüpft ist, wird die Datei „boot.ini“ in Editor geöffnet.




<!--HONumber=Oct16_HO1-->


