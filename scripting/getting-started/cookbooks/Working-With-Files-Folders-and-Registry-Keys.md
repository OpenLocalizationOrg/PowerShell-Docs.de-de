---
title: "Arbeiten mit Dateien, Ordnern und Registrierungsschlüsseln"
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: e6cf87aa-b5f8-48d5-a75a-7cb7ecb482dc
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 3e1bf444d7657b66422dab3eb8dbeef5e4d581b4

---

# Arbeiten mit Dateien, Ordnern und Registrierungsschlüsseln
Windows PowerShell verwendet das Nomen **Item** zum Verweisen auf Elemente in einem Windows PowerShell-Laufwerk. Im Zusammenhang mit dem Windows PowerShell FileSystem-Anbieter kann ein **Item** eine Datei, ein Ordner oder das Windows PowerShell-Laufwerk sein. Das Auflisten dieser Elemente und die Arbeiten damit ist in den meisten Verwaltungseinstellungen eine wichtige grundlegende Aufgabe. Daher sollen diese Aufgaben ausführlich erläutert werden.

### Auflisten von Dateien, Ordnern und Registrierungsschlüsseln (Get-ChildItem)
Da das Abrufen einer Sammlung von Elementen von einem bestimmten Standort eine sehr häufig vorkommende Aufgabe ist, wurde das Cmdlet **Get-ChildItem** speziell dazu entwickelt, alle in einem Container, z.B. einem Ordner, gefundenen Elemente zurückzugeben.

Wenn Sie alle Dateien und Ordner zurückgeben möchten, die direkt im Ordner „C:\Windows“ enthalten sind, geben Sie Folgendes ein:

```
PS> Get-ChildItem -Path C:\Windows
    Directory: Microsoft.Windows PowerShell.Core\FileSystem::C:\Windows
Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a---        2006-05-16   8:10 AM          0 0.log
-a---        2005-11-29   3:16 PM         97 acc1.txt
-a---        2005-10-23  11:21 PM       3848 actsetup.log
...
```

Die Auflistung gleicht dem, was Sie sehen, wenn Sie den Befehl **dir** in **Cmd.exe** oder den Befehl **ls** in einer UNIX-Befehlsshell eingeben.

Sie können unter Verwendung der Parameter des Cmdlets **Get-ChildItem** sehr komplexe Auflistungen erstellen. Wir werden als Nächstes einige Szenarien näher betrachten. Sie können die Syntax des Cmdlets **Get-ChildItem** anzeigen, indem Sie Folgendes eingeben:

```
PS> Get-Command -Name Get-ChildItem -Syntax
```

Diese Parameter können gemischt und angepasst werden, um eine stark angepasste Ausgabe zu erhalten.

#### Auflisten aller enthaltenen Elemente (-Recurse)
Um sowohl die Elemente in einem Windows-Ordner als auch alle in dessen Unterordnern enthaltenen Elemente anzuzeigen, verwenden Sie den Parameter **Recurse** von **Get-ChildItem**. Die Auflistung zeigt alles, was im Windows-Ordner enthalten ist, und alle Elemente in seinen Unterordnern an. Beispiel:

```
PS> Get-ChildItem -Path C:\WINDOWS -Recurse

    Directory: Microsoft.Windows PowerShell.Core\FileSystem::C:\WINDOWS
    Directory: Microsoft.Windows PowerShell.Core\FileSystem::C:\WINDOWS\AppPatch
Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a---        2004-08-04   8:00 AM    1852416 AcGenral.dll
...
```

#### Filtern von Elementen anhand des Namens (-Name)
Um nur die Elementnamen anzuzeigen, verwenden Sie den Parameter **Name** von **Get-Childitem**:

```
PS> Get-ChildItem -Path C:\WINDOWS -Name
addins
AppPatch
assembly
...
```

#### Erzwungenes Auflisten von ausgeblendeten Elementen (-Force)
Elemente, die normalerweise im Datei-Explorer oder in „Cmd.exe“ nicht sichtbar sind, werden in der Ausgabe des Befehls **Get-ChildItem** nicht angezeigt. Um ausgeblendete Elemente anzuzeigen, verwenden Sie den Parameter **Force** von **Get-ChildItem**. Beispiel:

```
Get-ChildItem -Path C:\Windows -Force
```

Dieser Parameter heißt „Force“ (Zwingen), weil Sie mit ihm erzwingen können, dass das normale Verhalten des Befehls **Get-ChildItem** außer Kraft gesetzt wird. „Force“ ist ein weit verbreiteter Parameter, der eine Aktion erzwingt, die ein Cmdlet normalerweise nicht ausführen würde. Allerdings wird keine Aktion ausgeführt, die die Sicherheit des Systems gefährden würde.

#### Abgleichen von Elementnamen mit Platzhaltern
Der Befehl **Get-ChildItem** akzeptiert Platzhalter im Pfad der aufzulistenden Elemente.

Da das Abgleichen von Platzhaltern vom Windows PowerShell-Modul durchgeführt wird, verwenden alle Cmdlets, die Platzhalter akzeptieren, die gleiche Notation und das gleiche Abgleichverhalten. Die Windows PowerShell-Notation für Platzhalter enthält Folgendes:

-   Sternchen (*) steht für null oder mehr beliebige Zeichen.

-   Fragezeichen (?) steht für genau ein Zeichen.

-   Die linke eckige Klammer ([) und die rechte eckige Klammer (]) umgeben eine Gruppe von Zeichen, die für den Abgleich verwendet werden soll.

Hier sind einige Beispiele für die Funktionsweise der Platzhalterspezifikation.

Um im Windows-Verzeichnis alle Dateien mit dem Suffix **.log** und genau fünf Zeichen im Basisnamen zu suchen, geben Sie den folgenden Befehl ein:

```
PS> Get-ChildItem -Path C:\Windows\?????.log
    Directory: Microsoft.Windows PowerShell.Core\FileSystem::C:\Windows
Mode                LastWriteTime     Length Name
----                -------------     ------ ----
...
-a---        2006-05-11   6:31 PM     204276 ocgen.log
-a---        2006-05-11   6:31 PM      22365 ocmsn.log
...
-a---        2005-11-11   4:55 AM         64 setup.log
-a---        2005-12-15   2:24 PM      17719 VxSDM.log
...
```

Um im Windows-Verzeichnis alle Dateien zu suchen, die mit dem Buchstaben **x** beginnen, geben Sie Folgendes ein:

```
Get-ChildItem -Path C:\Windows\x*
```

Um alle Dateien zu suchen, deren Name mit **x** oder **z** beginnt, geben Sie Folgendes ein:

```
Get-ChildItem -Path C:\Windows\[xz]*
```

#### Ausschließen von Elementen (-Exclude)
Sie können bestimmte Elemente durch Verwenden des Parameters **Exclude** von „Get-ChildItem“ ausschließen. Auf diese Weise können Sie eine komplexe Filterung in einer einzigen Anweisung durchführen.

Angenommen, Sie möchten die Windows Time Service-DLL im Ordner „System32“ suchen, und Sie können sich nur daran erinnern, dass der Name der DLL mit „W“ anfängt und „32“ darin enthalten ist.

Ein Ausdruck wie **W\ &#42; 32\ &#42;. DLL** findet alle DLLs, die die Bedingung erfüllen, aber Windows 95 und Windows-Kompatibilität mit 16-Bit-DLLs, die "95" enthalten oder "16" kann auch in ihren Namen zurück. Sie können Dateien ausschließen, die diese Zahlen in ihren Namen enthalten, indem Sie den Parameter **Exclude** mit dem Muster **&#42;[9516]&#42;** verwenden.

<pre>PS> Get-ChildItem -Path C:\WINDOWS\System32\w*32*.dll -Exclude *[9516]* Directory: Microsoft.PowerShell.Core\FileSystem::C:\WINDOWS\System32 Mode                LastWriteTime     Length Name ----                -------------     ------ ---- -a---        2004-08-04   8:00 AM     174592 w32time.dll -a---        2004-08-04   8:00 AM      22016 w32topl.dll -a---        2004-08-04   8:00 AM     101888 win32spl.dll -a---        2004-08-04   8:00 AM     172032 wldap32.dll -a---        2004-08-04   8:00 AM     264192 wow32.dll -a---        2004-08-04   8:00 AM      82944 ws2_32.dll -a---        2004-08-04   8:00 AM      42496 wsnmp32.dll -a---        2004-08-04   8:00 AM      22528 wsock32.dll -a---        2004-08-04   8:00 AM      18432 wtsapi32.dll</pre>

#### Kombinieren von Get-ChildItem-Parametern
Sie können verschiedene Parameter des Cmdlets **Get-ChildItem** im gleichen Befehl verwenden. Bevor Sie Parameter kombinieren, sollten Sie sicher sein, dass Sie das Abgleichen mit Platzhalterzeichen verstanden haben. Beispielsweise gibt der folgende Befehl keine Ergebnisse zurück:

```
PS> Get-ChildItem -Path C:\Windows\*.dll -Recurse -Exclude [a-y]*.dll
```

Es werden keine Ergebnisse zurückgegeben, obwohl es im Windows-Ordner zwei DLLs gibt, die mit dem Buchstaben „z“ beginnen.

Es wurden keine Ergebnisse zurückgegeben, weil das Platzhalterzeichen als Teil des Pfads angegeben wurde. Obwohl der Befehl rekursiv war, hat das Cmdlet **Get-ChildItem** die Elemente auf die Elemente im Windows-Ordner beschränkt, deren Namen mit „.dll“ enden.

Um eine rekursive Suche nach Dateien anzugeben, deren Namen einem bestimmten Muster entsprechen, verwenden Sie den Parameter **-Include**.

```
PS> Get-ChildItem -Path C:\Windows -Include *.dll -Recurse -Exclude [a-y]*.dll

    Directory: Microsoft.Windows PowerShell.Core\FileSystem::C:\Windows\System32\Setup

Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a---        2004-08-04   8:00 AM       8261 zoneoc.dll

    Directory: Microsoft.Windows PowerShell.Core\FileSystem::C:\Windows\System32

Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a---        2004-08-04   8:00 AM     337920 zipfldr.dll
```




<!--HONumber=Oct16_HO1-->


