---
title: "Verwenden von „Format“-Befehlen zum Ändern der Ausgabeanzeige"
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 63515a06-a6f7-4175-a45e-a0537f4f6d05
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 4d8c2392c6a9f59dc5e601308bc08db8c653760e

---

# Verwenden von „Format“-Befehlen zum Ändern der Ausgabeanzeige
Windows PowerShell hat eine Reihe von Cmdlets, mit denen Sie steuern können, welche Eigenschaften für bestimmte Objekte angezeigt werden. Der Name jedes dieser Cmdlets beginnt mit dem Verb **Format**. In diesen Cmdlets können Sie eine oder mehrere anzuzeigende Eigenschaften auswählen.

Die **Format**-Cmdlets sind**Format-Wide**, **Format-List**, **Format-Table** und **Format-Custom**. In dieser Benutzeranleitung werden nur die Cmdlets **Format-Wide**, **Format-List** und **Format-Table** beschrieben.

Jedes „Format“-Cmdlet hat Standardeigenschaften, die verwendet werden, wenn Sie keine bestimmten anzuzeigenden Eigenschaften angeben. Für jedes Cmdlet wird außerdem derselbe Parametername, **Property**, verwendet, um anzugeben, welche Eigenschaften angezeigt werden sollen. Weil **Format-Wide** nur eine einzelne Eigenschaft anzeigt, übernimmt deren **Property**-Parameter nur einen einzelnen Wert, wogegen die „Property“-Parameter von **Format-List** und **Format-Table** eine Liste von Eigenschaftsnamen akzeptieren.

Wenn Sie den Befehl **Get-Process -Name powershell** verwenden, wenn zwei Instanzen von Windows PowerShell ausgeführt werden, erhalten Sie eine Ausgabe, die folgendermaßen aussieht:

```
Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
    995       9    30308      27996   152     2.73   2760 powershell
    331       9    23284      29084   143     1.06   3448 powershell
```

Im Rest dieses Abschnitts wird untersucht, wie mit **Format**-Cmdlets die Art und Weise der Ausgabeanzeige dieses Befehl geändert werden kann.

### Verwenden von „Format-Wide“ für die Ausgabe eines einzelnen Elements
Das Cmdlet **Format-Wide** zeigt standardmäßig nur die Standardeigenschaft eines Objekts an. Die Informationen, die zu dem jeweiligen Objekt gehören, werden in einer einzelnen Spalte angezeigt:

```
PS> Get-Process -Name powershell | Format-Wide

powershell                              powershell
```

Sie können auch eine nicht standardmäßige Eigenschaft angeben:

```
PS> Get-Process -Name powershell | Format-Wide -Property Id

2760                                    3448
```

#### Steuern der „Format-Wide“-Anzeige mit Spalte
Mit dem Cmdlet **Format-Wide** können Sie immer nur jeweils eine Eigenschaft anzeigen. Dadurch ist es zum Anzeigen einfacher Listen geeignet, in denen nur ein Element pro Zeile aufgeführt wird. Um eine einfache Liste zu erhalten, legen Sie den Wert des **Column**-Parameter auf 1 fest, indem Sie Folgendes eingeben:

```
Get-Command Format-Wide -Property Name -Column 1
```

### Verwenden von „Format-List“ für eine Listenansicht
Das Cmdlet **Format-List** zeigt ein Objekt in Form einer Auflistung an, wobei jede Eigenschaft in einer eigenen Zeile beschriftet und angezeigt wird:

```
PS> Get-Process -Name powershell | Format-List

Id      : 2760
Handles : 1242
CPU     : 3.03125
Name    : powershell

Id      : 3448
Handles : 328
CPU     : 1.0625
Name    : powershell
```

Sie können Sie beliebig viele Eigenschaften angeben:

```
PS> Get-Process -Name powershell | Format-List -Property ProcessName,FileVersion
,StartTime,Id

ProcessName : powershell
FileVersion : 1.0.9567.1
StartTime   : 2006-05-24 13:42:00
Id          : 2760

ProcessName : powershell
FileVersion : 1.0.9567.1
StartTime   : 2006-05-24 13:54:28
Id          : 3448
```

#### Abrufen von ausführliche Informationen durch Verwenden von „Format-List“ mit Platzhaltern
Das Cmdlet **Format-List** bietet Ihnen die Möglichkeit, einen Platzhalters als Wert für den **Property**-Parameter zu verwenden. Dies ermöglicht es Ihnen, ausführliche Informationen anzuzeigen. Häufig enthalten Objekte mehr Informationen, als Sie benötigen. Dies ist der Grund, warum Windows PowerShell nicht standardmäßig alle Eigenschaftswerte anzeigt. Um alle Eigenschaften eines Objekts anzuzeigen, verwenden Sie die **Format-List-Eigenschaft \ &#42;** Befehl. Der folgende Befehl generiert mehr als 60 Zeilen an Ausgabe für einen einzelnen Prozess:

```
Get-Process -Name powershell | Format-List -Property *
```

Obwohl der Befehl **Format-List** nützlich ist, Details anzuzeigen, ist eine einfachere tabellarische Ansicht oft nützlicher, wenn Sie einen Überblick über Ausgabe wünschen, die viele Elemente enthält.

### Verwenden von „Format-Table“ für tabellarische Ausgabe
Wenn Sie die Ausgabe des Befehls **Get-Process** mit dem Cmdlet **Format-Table** ohne angegebene Eigenschaftsnamen formatieren, erhalten Sie genau dieselbe Ausgabe wie für den Fall, dass Sie keine Formatierung anwenden. Der Grund hierfür ist, dass Prozesse in der Regel in einem Tabellenformat angezeigt werden, wie dies für die meisten Windows PowerShell-Objekte zutrifft.

```
PS> Get-Process -Name powershell | Format-Table

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
   1488       9    31568      29460   152     3.53   2760 powershell
    332       9    23140        632   141     1.06   3448 powershell
```

#### Verbessern der „Format-Table“-Ausgabe (AutoSize)
Eine tabellarische Ansicht ist zwar zum Anzeigen vieler vergleichbarer Informationen nützlich, kann aber zu Interpretationsschwierigkeiten führen, wenn die Anzeige zu schmal für die Daten ist. Wenn Sie beispielsweise versuchen, den Pfad, die ID, den Namen und das Unternehmen eines Prozesses anzuzeigen, erhalten Sie abgeschnittene Ausgabe für die Spalten mit dem Pfad und dem Unternehmen des Prozesses:

```
PS> Get-Process -Name powershell | Format-Table -Property Path,Name,Id,Company

Path                Name                                 Id Company
----                ----                                 -- -------
C:\Program Files... powershell                         2836 Microsoft Corpor...
```

Wenn Sie den **AutoSize**-Parameter angeben, wenn Sie den Befehl **Format-Table** ausführen, berechnet Windows PowerShell die Spaltenbreiten anhand der tatsächlichen Daten, die Sie anzeigen möchten. Dadurch lässt sich die Spalte **Pfad** besser lesen, aber die Spalte für das Unternehmen bleibt abgeschnitten:

```
PS> Get-Process -Name powershell | Format-Table -Property Path,Name,Id,Company -
AutoSize

Path                                                    Name         Id Company
----                                                    ----         -- -------
C:\Program Files\Windows PowerShell\v1.0\powershell.exe powershell 2836 Micr...
```

Im **Format-Table**Cmdlet werden möglicherweise weiterhin Daten abgeschnitten, aber dies erfolgt am Ende der Bildschirmausgabe. Jede Eigenschaft außer der letzten angezeigten Eigenschaft erhält so viel Breite, wie erforderlich ist, damit ihr längstes Datenelement richtig angezeigt wird. Sie können sehen, dass der Unternehmensname vollständig sichtbar ist, der Pfad aber abgeschnitten wird, wenn Sie die Positionen von **Path** und **Company** in der **Property**-Werteliste tauschen:

```
PS> Get-Process -Name powershell | Format-Table -Property Company,Name,Id,Path -
AutoSize

Company               Name         Id Path
-------               ----         -- ----
Microsoft Corporation powershell 2836 C:\Program Files\Windows PowerShell\v1...
```

Für den Befehl **Format-Table** wird angenommen, dass eine Eigenschaft umso wichtiger ist, je näher sie sich am Anfang der Eigenschaftenliste befindet. Daher wird im Befehl versucht, die Eigenschaften, die dem Anfang am nächsten sind, vollständig anzuzeigen. Wenn der Befehl **Format-Table** nicht alle Eigenschaften anzeigen kann, entfernt er einige Spalten aus der Anzeige und gibt eine Warnung aus. Sie können dieses Verhalten sehen, wenn Sie **Name** zur letzten Eigenschaft in der Liste machen:

```
PS> Get-Process -Name powershell | Format-Table -Property Company,Path,Id,Name -
AutoSize

WARNING: column "Name" does not fit into the display and was removed.

Company               Path                                                    I
                                                                              d
-------               ----                                                    -
Microsoft Corporation C:\Program Files\Windows PowerShell\v1.0\powershell.exe 6
```

In dieser Ausgabe ist die ID-Spalte abgeschnitten, damit sie in die Auflistung passt, und die Spaltenüberschriften sind gestapelt. Eine automatische Größenänderung der Spalten erfolgt nicht immer so, wie Sie dies wünschen.

#### Umbrechen von „Format-Table“-Ausgabe in Spalten (Wrap)
Sie können erzwingen, dass zu lange **Format-Table**-Daten innerhalb ihrer Anzeigespalte umbrochen werden, indem Sie den **Wrap**-Parameter verwenden. Das alleinige Verwenden des **Wrap**-Parameters bringt nicht notwendigerweise das von Ihnen erwartete Ergebnis, weil dann die Standardeinstellungen verwendet werden, wenn Sie nicht auch **AutoSize** angeben:

```
PS> Get-Process -Name powershell | Format-Table -Wrap -Property Name,Id,Company,
Path

Name                                 Id Company             Path
----                                 -- -------             ----
powershell                         2836 Microsoft Corporati C:\Program Files\Wi
                                        on                  ndows PowerShell\v1
                                                            .0\powershell.exe
```

Ein Vorteil der Verwendung des **Wrap**-Parameters an sich besteht darin, dass er die Verarbeitung kaum verlangsamt. Wenn Sie eine rekursive Dateiauflistung eines umfangreichen Verzeichnissystems ausführen, kann es sehr lange dauern und viel Arbeitsspeicher erfordern, bis die ersten Ausgabeelemente angezeigt werden, wenn Sie **AutoSize** verwenden.

Wenn die Systemauslastung keine Rolle spielt, funktioniert **AutoSize** gut mit dem **Wrap**-Parameter. Den ersten Spalten werden immer die Breiten zugeteilt, die für sie benötigt werden, um Elemente in einer Zeile anzuzeigen, genau so, als würden Sie **AutoSize** ohne den **Wrap**-Parameter angeben. Der einzige Unterschied ist, dass die letzte Spalte ggf. umbrochen wird:

```
PS> Get-Process -Name powershell | Format-Table -Wrap -AutoSize -Property Name,I
d,Company,Path

Name         Id Company               Path
----         -- -------               ----
powershell 2836 Microsoft Corporation C:\Program Files\Windows PowerShell\v1.0\
                                      powershell.exe
```

Einige Spalten werden möglicherweise nicht angezeigt, wenn Sie die breitesten Spalten zuerst angeben. Daher ist es am sichersten, zunächst die schmalsten Datenelemente anzugeben. Im folgenden Beispiel ist das extrem breite „Path“-Element zuerst angegeben, und selbst mit Umbrechen geht die letzte **Name** Spalte verloren:

```
PS> Get-Process -Name powershell | Format-Table -Wrap -AutoSize -Property Path,I
d,Company,Name

WARNING: column "Name" does not fit into the display and was removed.

Path                                                      Id Company
----                                                      -- -------
C:\Program Files\Windows PowerShell\v1.0\powershell.exe 2836 Microsoft Corporat
                                                             ion
```

#### Ordnen von Tabellenausgabe (-GroupBy)
Eine weiterer nützlicher Parameter für das Steuern von tabellarischer Ausgabe ist **GroupBy**. Es kann insbesondere schwierig sein, längere tabellarische Auflistungen zu vergleichen. Mit dem **GroupBy**Parameter wird die Ausgabe anhand eines Eigenschaftswerts gruppiert. Beispielsweise können Prozesse zur einfacheren Überprüfung nach dem Unternehmen gruppiert werden, wobei der Unternehmenswert aus der Eigenschaftenliste entfernt ist:

```
PS> Get-Process -Name powershell | Format-Table -Wrap -AutoSize -Property Name,I
d,Path -GroupBy Company

   Company: Microsoft Corporation

Name         Id Path
----         -- ----
powershell 1956 C:\Program Files\Windows PowerShell\v1.0\powershell.exe
powershell 2656 C:\Program Files\Windows PowerShell\v1.0\powershell.exe
```




<!--HONumber=Oct16_HO1-->


