---
title: Grundlegendes zu wichtigen Windows PowerShell-Konzepten
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 3e601e38-4520-4578-a48d-b6779f1d35ee
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 89634d992eb59650e1f1a6cf89064b477ac4fea9

---

# Grundlegendes zu wichtigen Windows PowerShell-Konzepten
Für Windows PowerShell wurden Konzepte aus vielen verschiedenen Umgebungen integriert. Mit mehreren davon sind Personen vertraut, die Erfahrung mit bestimmten Shells oder Programmierumgebungen haben, doch nur sehr wenige kennen alle. Ein Blick auf einige dieser Konzepte ermöglicht eine nützliche Übersicht über die Shell.

### Befehle sind nicht textbasiert
Im Gegensatz zu herkömmlichen Befehlszeilenschnittstellen sind Windows PowerShell-Cmdlets auf das Verarbeiten von Objekten ausgelegt. Dabei handelt es sich um strukturierte Informationen, die mehr sind als bloß eine Zeichenfolge, die auf dem Bildschirm angezeigt wird. Die Befehlsausgabe enthält stets zusätzliche Informationen, die Sie bei Bedarf nutzen können. In diesem Dokument wird dieses Thema ausführlich erläutert.

Wenn Sie bislang Textverarbeitungstools zum Verarbeiten von Befehlszeilendaten genutzt haben, werden Sie feststellen, dass sie sich anders verhalten, wenn Sie versuchen, sie in Windows PowerShell zu verwenden. In den meisten Fällen benötigen Sie keine Textverarbeitungstools, um bestimmte Informationen zu extrahieren. Über standardmäßige Windows PowerShell-Objektbearbeitungsbefehle können Sie auf Teile der Daten direkt zugreifen.

### Die Befehlsfamilie ist erweiterbar
Schnittstellen wie „Cmd.exe“ bieten keine Möglichkeit, den integrierten Befehlssatz direkt zu erweitern. Sie können externe Befehlszeilentools erstellen, die in „Cmd.exe“ ausgeführt werden. Doch diese externen Tools bieten keinen Dienste, z.B. die Integration von Hilfe, und „Cmd.exe“ weiß nicht automatisch, ob es sich um gültige Befehle handelt.

Die nativen binären Befehle in Windows PowerShell, die *Cmdlets* (ausgesprochen „Commandlets“) genannt werden, können mithilfe von Cmdlets verbessert werden, die Sie mithilfe von Snap-Ins erstellen und Windows PowerShell hinzufügen. Windows PowerShell-*Snap-Ins* werden wie binäre Tools in einer beliebigen anderen Schnittstelle kompiliert. Mit ihrer Hilfe können Sie Windows PowerShell-Anbieter der Shell und auch neue Cmdlets hinzufügen.

Aufgrund der Besonderheit der internen Windows PowerShell-Befehle bezeichnen wir diese als *Cmdlets*.

> [!NOTE]
> Windows PowerShell kann andere Befehle als Cmdlets ausführen. Im Windows PowerShell-Benutzerhandbuch gehen wir nicht näher auf diese ein, doch es ist nützlich, sie als Kategorien von Befehlstypen zu kennen. Windows PowerShell unterstützt Skripts, die mit UNIX-Shellskripts und „Cmd.exe“-Batchdateien vergleichbar sind, aber die Erweiterung „.ps1“ haben. Windows PowerShell ermöglicht auch das Erstellen interner Funktionen, die direkt an der Schnittstelle oder in Skripts verwendet werden können.

### Windows PowerShell übernimmt die Konsoleneingabe und -anzeige
Wenn Sie einen Befehl eingeben, verarbeitet Windows PowerShell die Befehlszeile stets direkt. Windows PowerShell formatiert auch die Ausgabe, die Sie auf dem Bildschirm sehen. Dies ist wichtig, da dadurch der Aufwand für jedes Cmdlet reduziert und sichergestellt wird, dass Sie Aufgaben unabhängig vom verwendeten Cmdlet stets auf dieselbe Weise ausführen können. Ein Beispiel dafür, wie dadurch das Leben für Entwickler und Benutzern von Tools vereinfacht wird, ist die Hilfe zur Befehlszeile.

Herkömmliche Befehlszeilentools haben eigene Schemas zum Anfordern und Anzeigen von Hilfe. Bei einigen Befehlszeilentools muss **/?** eingegeben werden, um die Anzeige von Hilfe auszulösen. Bei anderen muss **-?**, **/H** oder gar **//** eingegeben werden. Bei einigen wird Hilfe in einem Fenster auf der grafischen Benutzeroberfläche und nicht in der Konsole angezeigt. Einige komplexe Tools, z. B. Tools für die Anwendungsaktualisierung, entpacken interne Dateien vor der Anzeige ihrer Hilfe. Wenn Sie den falschen Parameter verwenden, ignoriert das Tool ggf., was Sie eingegeben haben, und beginnt, einen Task automatisch auszuführen.

Wenn Sie einen Befehl in Windows PowerShell eingeben, wird alles, was Sie eingeben, von Windows PowerShell automatisch analysiert und vorverarbeitet. Bei Verwendung des Parameters **-?** mit einem Windows PowerShell-Cmdlet, bedeutet dies immer „Hilfe für diesen Befehl anzeigen“. Cmdlet-Entwickler müssen den Befehl nicht analysieren, sondern lediglich den Hilfetext bereitstellen.

Wichtig ist der Hinweis, dass die Hilfefeatures von Windows PowerShell auch dann verfügbar sind, wenn Sie in Windows PowerShell herkömmliche Befehlszeilentools ausführen. Windows PowerShell verarbeitet die Parameter und übergibt die Ergebnisse an die externen Tools.

> [!NOTE]
> Wenn Sie eine grafische Anwendung in Windows PowerShell ausführen, wird das Fenster der Anwendung geöffnet. Windows PowerShell greift nur ein, um die von Ihnen angegebene Befehlszeileneingabe oder die vom Konsolenfenster zurückgegebene Anwendungsausgabe zu verarbeiten. Die internen Arbeitsabläufe der Anwendung werden nicht beeinflusst.

### Windows PowerShell verwendet C#-Syntax
Windows PowerShell verfügt über Syntaxfeatures und Schlüsselwörter, die denen der Programmiersprache C# sehr ähnlich sind, da Windows PowerShell auf .NET Framework basiert. Das Erlernen von Windows PowerShell vereinfacht das Erlernen von C# wesentlich, wenn Sie an dieser Sprache interessiert sein sollten.

Wenn Sie kein C#-Programmierer sind, ist diese Ähnlichkeit ohne Bedeutung. Wenn Sie allerdings bereits mit C# vertraut sind, ist das Erlernen von Windows PowerShell aufgrund der Ähnlichkeiten viel einfacher.




<!--HONumber=Oct16_HO1-->


