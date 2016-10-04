# Verbesserungen beim Debuggen von PowerShell-Skripts

Für PowerShell 5.0 wurden zahlreiche Verbesserungen vorgenommen, um das Debuggen zu verbessern:

## Alle unterbrechen

Die PowerShell-Konsole und Windows PowerShell ISE ermöglichen Ihnen nun bei der Ausführung von Skripts, den Debugger zu unterbrechen. Dies funktioniert in lokalen und Remotesitzungen.

Drücken Sie in der Konsole **STRG+UNTBR**.

Drücken Sie in der ISE **STRG+B,**, oder verwenden Sie den Menübefehl **Debuggen -> Alle unterbrechen**.

## Remotedebuggen und Remotedateibearbeitung in der Windows PowerShell ISE

Die Windows PowerShell ISE ermöglicht nun das Öffnen und Bearbeiten von Dateien in einer Remotesitzung, indem der Befehl „PSEdit“ ausgeführt wird.
Sie können z. B. eine Datei zur Bearbeitung in der Befehlszeile in einer Remotesitzung wie folgt öffnen:

```powershell
[RemoteComputer1]: PS C:\> PSEdit C:\DebugDemoScripts\Test-GetMutex.ps1
```

Darüber hinaus können Sie nun Bearbeitungen vornehmen und Änderungen in einer Remotedatei speichern, die in Windows PowerShell ISE automatisch geöffnet wird, wenn ein Haltepunkt erreicht wird.
Sie können nun eine Skriptdatei debuggen, die auf einem Remotecomputer ausgeführt wird, die Datei bearbeiten, um einen Fehler zu beheben, und dann das geänderte Skript erneut ausführen.

## Erweitertes Skriptdebugging

Es gibt neue, erweiterte Debugfunktionen, die an beliebige lokale Computerprozesse angefügt werden können, für die Windows PowerShell geladen ist, und beliebige Runspaces im jeweiligen Prozess debuggen.

### Debuggen von Runspaces

Neue Cmdlets wurden hinzugefügt, mit denen Sie aktuelle Runspaces in einem Prozess auflisten und die Windows PowerShell-Konsole oder ISE-Debugger an den jeweiligen Runspace zum Debuggen von Skripts anfügen können:

-   Get-Runspace
-   Debug-Runspace
-   Enable-RunspaceDebug
-   Disable-RunspaceDebug
-   Get-RunspaceDebug

### Anfügen an den Prozess, der PowerShell hostet

Nun ist ein Anfügen an jeden Computerprozess möglich, für den Windows PowerShell geladen wurde. Starten Sie dazu eine interaktive Sitzung mit dem Prozess ähnlich dem Starten einer interaktiven Remotesitzung, indem Sie das Cmdlet „Enter-PSSession“ ausführen:

-   Enter-PSHostProcess
-   Exit-PSHostProcess

<!--HONumber=Oct16_HO1-->


