---
title: Umleiten von Daten mit Out   Cmdlets
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 2a4acd33-041d-43a5-a3e9-9608a4c52b0c
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: e62ae14e4d7334d0cd42681c7e2591692c089187

---

# Umleiten von Daten mit Out-*-Cmdlets
Windows PowerShell stellt mehrere Cmdlets bereit, mit denen Sie die Datenausgabe direkt steuern können. Diese Cmdlets haben zwei wichtige Merkmale gemeinsam.

Zum Ersten transformieren sie Daten grundsätzlich in eine Form von Text. Dies geschieht, weil die Cmdlets Daten an Systemkomponenten ausgegeben, die Texteingabe erfordern. Das heißt, sie müssen die Objekte als Text darstellen. Daher wird der Text so formatiert, wie Sie ihn im Windows PowerShell-Konsolenfenster sehen.

Zum Zweiten wird für diese Cmdlets das Windows PowerShell-Verb **Out** verwendet, weil sie Informationen aus Windows PowerShell heraus an ein anderes Element senden. Das Cmdlet **Out-Host** ist keine Ausnahme: Die Hostfensteranzeige befindet sich außerhalb der Windows PowerShell. Dies ist wichtig, denn Daten, die aus Windows PowerShell gesendet werden, werden tatsächlich entfernt. Sie können dies sehen, wenn Sie eine Pipeline erstellen, über die die Daten an das Hostfenster ausgelagert werden, und dann versuchen, die Daten als Liste zu formatieren, wie hier gezeigt:

```
PS> Get-Process | Out-Host -Paging | Format-List
```

Sie erwarten wahrscheinlich, dass der Befehl Seiten mit Prozessinformationen im Listenformat anzeigt. Stattdessen werden die Daten in der standardmäßigen tabellarischen Liste angezeigt:

```
Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
    101       5     1076       3316    32     0.05   2888 alg
...
    618      18    39348      51108   143   211.20    740 explorer
    257       8     9752      16828    79     3.02   2560 explorer
...
<SPACE> next page; <CR> next line; Q quit
...
```

Das Cmdlet **Out-Host** sendet die Daten direkt an die Konsole, sodass der Befehl **Format-List** nichts empfängt, was formatiert werden kann.

Damit dieser Befehl die richtige Struktur hat, muss das Cmdlet **Out-Host** an das Ende der Pipeline gesetzt werden, wie unten dargestellt. Dies bewirkt, dass die Prozessdaten in einer Liste formatiert werden, bevor sie ausgelagert und angezeigt werden.

```
PS> Get-Process | Format-List | Out-Host -Paging

Id      : 2888
Handles : 101
CPU     : 0.046875
Name    : alg
...

Id      : 740
Handles : 612
CPU     : 211.703125
Name    : explorer

Id      : 2560
Handles : 257
CPU     : 3.015625
Name    : explorer
...
<SPACE> next page; <CR> next line; Q quit
...
```

Dies gilt für alle **Out**-Cmdlets. Ein **Out**-Cmdlet sollte immer am Ende einer Pipeline stehen.

> [!NOTE]
> Alle **Out**-Cmdlets rendern Ausgabe als Text, wobei sie die Formatierung verwenden, die für das Konsolenfenster wirksam ist, einschließlich Längenbeschränkungen für Zeilen.

#### Auslagern von Konsolenausgabe (Out-Host)
Standardmäßig sendet Windows PowerShell Daten an das Hostfenster, und dies entspricht exakt der Funktionsweise des „Out-Host“-Cmdlets. Die vorrangige Verwendung für das „Out-Host“-Cmdlet ist das Auslagern von Daten, wie bereits erläutert. Beispielsweise wird „Out-Host“ im folgenden Befehl dazu verwendet,die Ausgabe des „Get-Command“-Cmdlets auszulagern:

```
PS> Get-Command | Out-Host -Paging
```

Sie können auch die **more**-Funktion verwenden, um Daten auszulagern. In Windows PowerShell ist **more** eine Funktion, die **Out-Host -Paging** aufruft. Mit dem folgenden Befehl wird veranschaulicht, wie die **more**-Funktion verwendet wird, um die Ausgabe von „Get-Command“ auszulagern:

```
PS> Get-Command | more
```

Wenn Sie einen oder mehrere Dateinamen als Argumente für die „more“-Funktion angeben, werden die Dateien von der Funktion gelesen, und die Funktion lagert die Inhalte der Dateien an den Host aus:

```
PS> more c:\boot.ini
[boot loader]
timeout=5
default=multi(0)disk(0)rdisk(0)partition(1)\WINDOWS
[operating systems]
...
```

#### Verwerfen von Ausgabe (Out-Null)
Das **Out-Null**-Cmdlet verwirft sofort jegliche Eingabe, die es empfängt. Damit können Sie überflüssige Daten verwerfen, die Sie als Nebeneffekt des Ausführens eines Befehls erhalten. Wenn Sie den folgenden Befehl eingeben, erhalten Sie keinerlei Rückgabe von dem Befehl:

```
PS> Get-Command | Out-Null
```

Das Cmdlet **Out-Null** verwirft keine Fehlerausgabe. Wenn Sie beispielsweise den folgenden Befehl eingeben, wird eine Meldung angezeigt, der zu entnehmen ist, dass Windows PowerShell „Is-NotACommand“ nicht erkennt:

```
PS> Get-Command Is-NotACommand | Out-Null
Get-Command : 'Is-NotACommand' is not recognized as a cmdlet, function, operabl
e program, or script file.
At line:1 char:12
+ Get-Command  <<<< Is-NotACommand | Out-Null
```

#### Drucken von Daten (Out-Printer)
Sie können Daten drucken, indem Sie das Cmdlet **Out-Printer** verwenden. Das Cmdlet **Out-Printer** verwendet den Standarddrucker, wenn Sie keinen Druckernamen angeben. Sie können jeden Windows-basierten Drucker verwenden, indem Sie dessen Anzeigenamen angeben. Es sind weder irgendeine Art von Druckeranschlusszuordnung noch sogar ein echter physischer Drucker erforderlich. Wenn Sie etwa die Bilderstellungstools für Microsoft Office-Dokumente installiert haben, können Sie die Daten in eine Bilddatei senden, indem Sie Folgendes eingeben:

```
PS> Get-Command Get-Command | Out-Printer -Name "Microsoft Office Document Image Writer"
```

#### Speichern von Daten (Out-File)
Mit dem Cmdlet **Out-File** können Sie Ausgabe in eine Datei statt an das Konsolenfenster senden. In der folgenden Befehlszeile wird die jeweilige Liste der Prozesse in die Datei **C:\\temp\\processlist.txt** gesendet:

```
PS> Get-Process | Out-File -FilePath C:\temp\processlist.txt
```

Die Ergebnisse, die das Cmdlet **Out-File** bringt, entsprechen möglicherweise nicht Ihrer Erwartung, wenn Sie an herkömmliche Ausgabeumleitung denken. Um dessen Verhalten zu verstehen, müssen Sie den Kontext berücksichtigen, in dem das Cmdlet **Out-File** ausgeführt wird.

Standardmäßig erstellt das Cmdlet **Out-File** eine Unicode-Datei. Dies ist langfristig die beste Standardeinstellung, bedeutet aber, dass die Tools, die ASCII-Dateien erwarten, mit dem Standardausgabeformat nicht ordnungsgemäß funktionieren. Sie können das Standardausgabeformat in ASCII ändern, indem Sie den **Encoding**-Parameter verwenden:

```
PS> Get-Process | Out-File -FilePath C:\temp\processlist.txt -Encoding ASCII
```

**Out-File** formatiert Dateiinhalt so, dass er wie Konsolenausgabe aussieht. Dies bewirkt, dass die Ausgabe abgeschnitten wird, so wie dies in den meisten Fällen in einem Konsolenfenster erfolgt. Angenommen, Sie führen den folgenden Befehl aus:

```
PS> Get-Command | Out-File -FilePath c:\temp\output.txt
```

Die Ausgabe sieht folgendermaßen aus:

```
CommandType     Name                            Definition                     
-----------     ----                            ----------                     
Cmdlet          Add-Content                     Add-Content [-Path] <String[...
Cmdlet          Add-History                     Add-History [[-InputObject] ...
...
```

Wenn Sie Ausgabe erhalten möchten, die keine Zeilenumbrüche erzwingt, um die Bildschirmbreite einzuhalten, verwenden Sie den **Width**-Parameter, um die Breite anzugeben. Weil **Width** ein 32-Bit-Ganzzahl-Parameter ist, beträgt sein größter Wert 2147483647. Geben Sie Folgendes ein, um die Zeilenbreite auf diesen größten Wert festzulegen:

```
Get-Command | Out-File -FilePath c:\temp\output.txt -Width 2147483647
```

Das Cmdlet **Out-File** lässt sich dann am besten verwenden, wenn Sie Ausgabe so speichern möchten, wie sie in der Konsole angezeigt wird. Für eine genauere Steuerung des Ausgabeformats benötigen Sie Tools mit erweiterter Funktionalität. Diese Tools sind zusammen mit einigen Details zur Objektverarbeitung Gegenstand des nächsten Kapitels.




<!--HONumber=Oct16_HO1-->


