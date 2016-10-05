---
title: Grundlegendes zur Windows PowerShell-Pipeline
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 6be50926-7943-4ef7-9499-4490d72a63fb
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: d90bf940a1047b629f7b59d239aab50a78748251

---

# Grundlegendes zur Windows PowerShell-Pipeline
Die Weiterleitung über Pipes funktioniert nahezu überall in Windows PowerShell. Obwohl Text auf dem Bildschirm angezeigt wird, leitet Windows PowerShell Text nicht zwischen Befehlen weiter. Stattdessen werden Objekte weitergeleitet.

Die Notation für Pipelines ist ähnlich wie bei anderen Shells, weshalb es auf den ersten Blick möglicherweise nicht ersichtlich ist, dass in Windows PowerShell etwas Neues eingeführt wird. Wenn Sie beispielsweise das Cmdlet **Out-Host** zum Erzwingen einer Seitenanzeige der Ausgabe eines anderen Befehls verwenden, sieht die Ausgabe bloß wie der normale auf dem Bildschirm gezeigte Text aus, der in Seiten unterteilt ist:

```
PS> Get-ChildItem -Path C:\WINDOWS\System32 | Out-Host -Paging

    Directory: Microsoft.Windows PowerShell.Core\FileSystem::C:\WINDOWS\system32

Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a---        2005-10-22  11:04 PM        315 $winnt$.inf
-a---        2004-08-04   8:00 AM      68608 access.cpl
-a---        2004-08-04   8:00 AM      64512 acctres.dll
-a---        2004-08-04   8:00 AM     183808 accwiz.exe
-a---        2004-08-04   8:00 AM      61952 acelpdec.ax
-a---        2004-08-04   8:00 AM     129536 acledit.dll
-a---        2004-08-04   8:00 AM     114688 aclui.dll
-a---        2004-08-04   8:00 AM     194048 activeds.dll
-a---        2004-08-04   8:00 AM     111104 activeds.tlb
-a---        2004-08-04   8:00 AM       4096 actmovie.exe
-a---        2004-08-04   8:00 AM     101888 actxprxy.dll
-a---        2003-02-21   6:50 PM     143150 admgmt.msc
-a---        2006-01-25   3:35 PM      53760 admparse.dll
<SPACE> next page; <CR> next line; Q quit
...
```

Der Befehl „Out-Host -Paging“ ist ein nützliches Pipelineelement, wenn Sie eine lange Ausgabe haben, die langsam angezeigt werden soll. Er ist besonders nützlich, wenn der Vorgang sehr CPU-intensiv ist. Da die Verarbeitung an das Cmdlet „Out-Host“ übertragen wird, sobald eine vollständige Seite für die Anzeige bereit ist, wird der Vorgang von Cmdlets, die sich in der Pipeline davor befinden, angehalten, bis die nächste Seite der Ausgabe verfügbar ist. Sie können dies überprüfen, wenn Sie den Windows Task-Manager zum Überwachen der CPU- und Arbeitsspeicherauslastung durch Windows PowerShell verwenden.

Führen Sie den folgenden Befehl: **C:\\Windows Get-ChildItem-Recurse**. Vergleichen die Verwendung CPU und Arbeitsspeicher, um diesen Befehl: **C:\\Windows Get-ChildItem-Recurse | Out-Host-Paging**. Was Sie auf dem Bildschirm sehen, ist Text, doch der Grund dafür ist, dass es erforderlich ist, Objekte im Konsolenfenster als Text darzustellen. Dies ist lediglich eine Darstellung dessen, was innerhalb von Windows PowerShell wirklich passiert. Nehmen Sie z.B. das Cmdlet „Get-Location“. Bei Eingabe von **Get-Location**, während Ihr aktueller Speicherort der Stamm von Laufwerk C ist, sehen Sie die folgende Ausgabe:

```
PS> Get-Location

Path
----
C:\
```

Wenn von Windows PowerShell Text weiterleitet wird, mit dem ein Befehl wie **Get-Location | Out-Host** aufgerufen wird, dann wird von **Get-Location** an **Out-Host** eine Gruppe von Zeichen entsprechend der Anzeige auf dem Bildschirm übergeben. Mit anderen Worten, würden Sie die Überschrifteninformationen ignorieren **out-Host** würde das Zeichen als erstes erhalten "**C"**, klicken Sie dann auf das Zeichen '**:'**, klicken Sie dann auf das Zeichen '**\\'**. Das Cmdlet **Out-Host** kann nicht die Bedeutung der Zeichen bestimmen, die vom Cmdlet **Get-Location** ausgegeben werden.

Anstatt Befehle in einer Pipeline mithilfe von Text kommunizieren zu lassen, verwendet Windows PowerShell Objekte. Aus Sicht eines Benutzers packen Objekte zusammengehörige Informationen in ein Format, welches das Bearbeiten der Informationen als Einheit und Extrahieren bestimmter Elemente vereinfacht, die Sie benötigen.

Der Befehl **Get-Location** gibt keinen Text zurück, der den aktuellen Pfad enthält. Er gibt ein Paket mit Informationen zurück, die als **PathInfo** -Objekt bezeichnet werden, das den aktuellen Pfad zusammen mit anderen Informationen enthält. Das Cmdlet **Out-Host** gibt dann dieses **PathInfo**-Objekt auf dem Bildschirm aus. Windows PowerShell entscheidet anschließend, welche Informationen basierend auf den geltenden Formatierungsregeln wie angezeigt werden.

Die vom Cmdlet **Get-Location** ausgegebenen Überschriftsinformationen werden erst am Ende des Vorgangs als Teil des Prozesses zur Formatierung der Daten für die Anzeige auf dem Bildschirm hinzugefügt. Auf dem Bildschirm sehen Sie eine Zusammenfassung der Informationen und keine vollständige Darstellung des Ausgabeobjekts.

Angenommen, ein Windows PowerShell-Befehl gibt mehr Informationen aus, als im Konsolenfenster angezeigt werden. Wie können wir die nicht sichtbaren Elemente abrufen? Wie zeigen Sie die zusätzlichen Daten an? Was ist, wenn Sie die Daten in einem anderen Format als in demjenigen anzeigen möchten, das Windows PowerShell normalerweise verwendet?

Im weiteren Verlauf dieses Kapitels wird erörtert, wie Sie die Struktur bestimmter Windows PowerShell-Objekte erkennen, bestimmte Elemente auswählen und für eine einfachere Anzeige formatieren und diese Informationen an alternative Ausgabeziele wie Dateien oder Drucker übermitteln können.




<!--HONumber=Oct16_HO1-->


