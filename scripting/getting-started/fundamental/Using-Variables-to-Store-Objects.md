---
title: Verwenden von Variablen, um Objekte zu speichern
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: b1688d73-c173-491e-9ba6-6d0c1cc852de
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 6216f3e1a766c57a7549a3e3b4fbe76d043a8a41

---

# Verwenden von Variablen, um Objekte zu speichern
Windows PowerShell arbeitet mit Objekten. In Windows PowerShell können Sie Variablen – eigentlich benannte Objekte – erstellen, um Ausgabe zur späteren Verwendung beizubehalten. Wenn Sie mit dem Verwenden von Variablen in anderen Shells vertraut sind, sollten Sie daran denken, dass Windows PowerShell-Variablen keine Texte, sondern Objekte sind.

Variablen werden immer mit dem Anfangszeichen $ angegeben und können jedes alphanumerische Zeichen und den Unterstrich in ihren Namen enthalten.

### Erstellen einer Variablen
Sie können eine Variablen erstellen, indem Sie einen zulässigen Variablennamen eingeben:

```
PS> $loc
PS>
```

Dies gibt kein Ergebnis zurück, weil **$loc** keinen Wert hat. Sie können im selben Schritt eine Variable erstellen und ihr einen Wert zuweisen. Windows PowerShell erstellt die Variable nur, wenn sie nicht vorhanden ist. Andernfalls weist Windows PowerShell der vorhandenen Variablen den angegebenen Wert zu. Um Ihren aktuellen Speicherort in der Variablen **$loc** zu speichern, geben Sie Folgendes ein:

```
$loc = Get-Location
```

Es wird keine Ausgabe angezeigt, wenn Sie diesen Befehl eingeben, weil die Ausgabe an „$loc“ gesendet wird In Windows PowerShell ist angezeigte Ausgabe ein Nebeneffekt der Tatsache, dass Daten, die nicht anderweitig weitergeleitet werden, immer an den Bildschirm gesendet werden. Durch Eingeben von „$loc“ wird Ihr aktueller Speicherort angezeigt:

```
PS> $loc

Path
----
C:\temp
```

Sie können **Get-Member** verwenden, um Informationen über die Inhalte der Variablen anzuzeigen. Wenn Sie „$loc“ in einer Pipeline an „Get-Member“ übergeben, sehen Sie, dass die Variable ein **PathInfo**-Objekt ist, genau wie die Ausgabe von „Get-Location“:

```
PS> $loc | Get-Member -MemberType Property

   TypeName: System.Management.Automation.PathInfo

Name         MemberType Definition
----         ---------- ----------
Drive        Property   System.Management.Automation.PSDriveInfo Drive {get;}
Path         Property   System.String Path {get;}
Provider     Property   System.Management.Automation.ProviderInfo Provider {...
ProviderPath Property   System.String ProviderPath {get;}
```

### Verwalten von Variablen
Windows PowerShell stellt mehrere Befehle bereit, mit denen Variablen verwaltet werden können. Eine vollständige Liste in einem lesbaren Format erhalten Sie, wenn Sie Folgendes eingeben:

```
Get-Command -Noun Variable | Format-Table -Property Name,Definition -AutoSize -Wrap
```

Zusätzlich zu den Variablen, die Sie in Ihrer aktuellen Windows PowerShell-Sitzung erstellen, gibt es mehrere systemdefinierte Variablen. Mit dem Cmdlet **Remove-Variable** können Sie alle Variablen löschen, die nicht von Windows PowerShell kontrolliert werden. Geben Sie den folgenden Befehl ein, um alle Variablen zu löschen:

```
Remove-Variable -Name * -Force -ErrorAction SilentlyContinue
```

Daraufhin wird die nachstehende Bestätigungsaufforderung angezeigt.

```
Confirm
Are you sure you want to perform this action?
Performing operation "Remove Variable" on Target "Name: Error".
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help
(default is "Y"):A
```

Wenn Sie nun das Cmdlet **Get-Variable** ausführen, sehen Sie die verbliebenen Windows PowerShell-Variablen. Da es auch ein Windows PowerShell-Laufwerk für Variablen gibt, können Sie alle Windows PowerShell-Variablen ebenfalls durch die folgende Eingabe anzeigen:

```
Get-ChildItem variable:
```

### Verwenden von „Cmd.exe“-Variablen
Windows PowerShell ist zwar nicht „Cmd.exe“, wird aber trotzdem in einer Befehlsshellumgebung ausgeführt und kann die Variablen verwenden, die in jeder Umgebung in Windows verfügbar sind. Diese Variablen werden über ein Laufwerk namens **env:** verfügbar gemacht. Sie können diese Variablen anzeigen, indem Sie Folgendes eingeben:

```
Get-ChildItem env:
```

Obwohl die Standardcmdlets für Variablen nicht so konzipiert sind, dass sie mit **env:** funktionieren, können Sie sie weiterhin verwenden, indem Sie das Präfix **env:** angeben. Wenn Sie beispielsweise das Stammverzeichnis des Betriebssystems anzeigen möchten, können Sie die Befehlsshellvariable **%SystemRoot%** in Windows PowerShell verwenden, indem Sie Folgende eingeben:

```
PS> $env:SystemRoot
C:\WINDOWS
```

Sie können auch Umgebungsvariablen in Windows PowerShell erstellen und ändern. Für Umgebungsvariablen, auf die über Windows PowerShell zugegriffen wird, gelten die normalen Regeln für Umgebungsvariablen wie an sonstigen Stellen in Windows.




<!--HONumber=Oct16_HO1-->


