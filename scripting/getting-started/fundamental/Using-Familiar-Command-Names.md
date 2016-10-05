---
title: Verwenden von vertrauten Befehlsnamen
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 021e2424-c64e-4fa5-aa98-aa6405758d5d
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: b567510b9a39bfa62e64e62752a1a7d2002cf6b9

---

# Verwenden von vertrauten Befehlsnamen
Durch Verwenden eines Mechanismus namens *Aliasing* versetzt Windows PowerShell Benutzer in die Lage, über alternative Namen auf Befehle zu verweisen. Aliasing ermöglicht es Benutzern mit Erfahrung mit anderen Shells, übliche Befehlsnamen, die sie bereits kennen, wiederzuverwenden, um gleiche Vorgänge in Windows PowerShell auszuführen. Windows PowerShell-Aliase werden zwar nicht im Detail erläutert, Sie können diese aber weiterhin verwenden, wenn Sie sich mit Windows PowerShell vertraut machen.

Aliasing ordnet einen Befehlsnamen, den Sie eingeben, einem anderen Befehl zu. Windows PowerShell hat beispielsweise eine interne Funktion namens **Clear-Host**, die den Inhalt des Ausgabefensters löscht. Wenn Sie entweder den Befehl **cls** oder den Befehl **clear** an einer Eingabeaufforderung eingeben, interpretiert Windows PowerShell, dass dies ein Alias für die **Clear-Host**-Funktion ist, und führt die **Clear-Host**-Funktion aus.

Dieses Feature erleichtert Benutzern das Lernen von Windows PowerShell. Der erste Punkt ist, dass die meisten „Cmd.exe“- und UNIX-Benutzer ein großes Repertoire an Befehlen haben, die sie bereits anhand des Namens kennen. Und obwohl die Windows PowerShell-Gegenstücke möglicherweise keine identischen Ergebnisse erzielen, sind sie in der Form ähnlich genug, sodass Benutzer mit ihnen arbeiten können, ohne sich zuerst die Windows PowerShell-Namen einzuprägen. Der zweite Punkt ist, dass die Hauptquelle der Frustration beim Lernen einer neuen Shell, wenn der Benutzer bereits mit einer anderen Shell vertraut ist, die Fehler sind, die durch „Fingererinnerung“ verursacht werden. Wenn Sie jahrelang „Cmd.exe“ verwendet haben und den Inhalt eines mit Ausgabe gefüllten Bildschirms löschen möchten, werden Sie reflexartig den Befehl **cls** eingeben und die EINGABETASTE drücken. Ohne den Alias für die **Clear-Host**-Funktion in Windows PowerShell würden Sie schlicht die Fehlermeldung **"cls"wird nicht als Cmdlet, Funktion, ausführbares Programm oder Skriptdatei erkannt** erhalten. und ohne Idee zurückgelassen, wie Sie vorgehen müssen, um die Ausgabe zu löschen.

In der folgenden Liste sind die allgemeinen „Cmd.exe“- und UNIX-Befehle zusammengestellt, die Sie in Windows PowerShell verwenden können:

|||||
|-|-|-|-|
|cat|dir|mount|rm|
|cd|echo|move|rmdir|
|chdir|erase|popd|sleep|
|clear|h|ps|sort|
|cls|history|pushd|tee|
|copy|kill|pwd|type|
|del|lp|r|write|
|diff|ls|ren||

Wenn Sie feststellen, dass Sie einige dieser Befehle reflexartig verwenden, und die tatsächlichen Namen der systemeigenen Windows PowerShell-Befehle erfahren möchten, verwenden Sie den Befehl **Get-Alias**:

```
PS> Get-Alias cls

CommandType     Name                            Definition
-----------     ----                            ----------
Alias           cls                             Clear-Host
```

Damit Beispiele besser lesbar sind, wird es im Windows PowerShell-Benutzerhandbuch grundsätzlich vermieden, Aliase zu verwenden. Es kann jedoch hilfreich sein, dass Sie frühzeitig mehr über Aliase wissen, wenn Sie mit beliebigen Ausschnitten von Windows PowerShell-Code aus einer anderen Quelle arbeiten oder eigene Aliase definieren möchten. Im weiteren Verlauf dieses Abschnitts werden Standardaliase erläutert und wird gezeigt, wie Sie eigene Aliase definieren.

### Interpretieren von Standardaliasen
Im Gegensatz zu den oben beschriebenen Aliasen, die zur Namenskompatibilität mit anderen Schnittstellen entwickelt wurden, sind die in Windows PowerShell integrierten Aliase grundsätzlich als Abkürzungen vorgesehen. Diese kürzeren Namen können schnell eingegeben werden, lassen sich jedoch unmöglich lesen, wenn Sie nicht wissen, worauf sie verweisen.

In Windows PowerShell wird versucht, einen Kompromiss zwischen Klarheit und Knappheit zu finden, indem ein Satz von Standardaliasen bereitgestellt wird, die auf Kurzschriftnamen für allgemeine Verben und Substantive basieren. Dies ermöglicht einen Kernsatz von Aliasen für allgemeine Cmdlets, die lesbar sind, wenn Sie die Kurzschriftnamen wissen. Beispielsweise sind in Standardaliasen das Verb **Get** mit **g**, das Verb **Set** mit **s**, das Substantiv **Item** mit **i**, das Substantiv **Location** mit **l** und das Substantiv Command mit **cm** abgekürzt.

Es folgen kurze Beispiel, um zu veranschaulichen, wie dies funktioniert. Der Standardalias für „Get-Item“ ergibt sich aus der Kombination von **g** für Get und **i** für Item: **gi**. Der Standardalias für „Set-Item“ ergibt sich aus der Kombination von **s** für Set und **i** für Item: **si**. Der Standardalias für „Get-Location“ ergibt sich aus der Kombination von **g** für Get und **l** für Location: **gl**. Der Standardalias für „Set-Location“ ergibt sich aus der Kombination von **s** für Set und **l** für Location: **l**. Der Standardalias für „Get-Command“ ergibt sich aus der Kombination von **g** für Get und **cm** für Command: **gcm**. Es gibt kein „Set-Command“-Cmdlet, aber würde es das geben, können Sie erraten, dass sich der Standardalias aus **s** für Set und **cm** für Command ergeben würde: **scm**. Darüber hinaus könnten Personen, die mit Windows PowerShell-Aliasing vertraut sind und auf **scm** stoßen, erraten, dass sich der Alias auf „Set-Command“ bezieht.

### Erstellen neue Aliase
Mit dem Cmdlet „Set-Alias“ Sie können Sie Ihre eigenen Aliase erstellen. Beispielsweise werden in den folgenden Anweisungen die Cmdlet-Standardaliase erstellt, die unter „Interpretieren von Standardaliasen“ erläutert sind:

```
Set-Alias -Name gi -Value Get-Item
Set-Alias -Name si -Value Set-Item
Set-Alias -Name gl -Value Get-Location
Set-Alias -Name sl -Value Set-Location
Set-Alias -Name gcm -Value Get-Command
```

Intern verwendet Windows PowerShell Befehle wie diese während des Starts, aber diese Aliase sind nicht änderbar. Wenn Sie versuchen, einen dieser Befehle tatsächlich auszuführen, erhalten Sie einen Fehler, in dem erläutert wird, dass der Alias nicht geändert werden kann. Beispiel:

<pre>PS> Set-Alias -Name gi -Value Get-Item Set-Alias: Der Alias kann nicht geschrieben werden, da der Alias „gi“ schreibgeschützt oder konstant ist und daher nicht geschrieben werden kann.
At line:1 char:10 + Set-Alias  <<<< -Name gi -Value Get-Item</pre>




<!--HONumber=Oct16_HO1-->


