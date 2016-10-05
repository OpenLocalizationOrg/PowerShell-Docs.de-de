# Save-Script

Mit dem Cmdlet „Save-Script“ können Sie die Skriptdatei überprüfen, indem Sie sie an einem angegebenen Speicherort speichern.

## Beschreibung

Das Save-Skript-Cmdlet wird das angegebene Skript gespeichert.

## Cmdlet-syntax

```powershell
Get-Command -Name Save-Script -Module PowerShellGet -Syntax
```
## Online-Hilfe-Cmdlet-Referenz

[Save-Skript](http://go.microsoft.com/fwlink/?LinkId=619786)

## Beispiele für Befehle

### Beispiel 1: Speichern Sie ein Skript aus einem repository
Dieser Befehl speichert die neueste Version des Skripts Fabrikam-ClientScript aus GalleryINT Repository für den lokalen Ordner C:\ScriptSharingDemo

```powershell
Save-Script -Name Fabrikam-ClientScript -Repository GalleryINT -Path C:\ScriptSharingDemo
```

### Beispiel 2: Speichern einer Version eines Skripts durch Weiterleiten des suchen-Skript-Cmdlets

Der erste Befehl sucht nach Version 1.5 der Fabrikam-ClientScript aus dem Repository GalleryINT und speichert diese in den Ordner C:\ScriptSharingDemo

Der zweite Befehl überprüft den Metadaten gespeichertes Skript.

```powershell
Find-Script -Name Fabrikam-ClientScript -Repository GalleryINT -RequiredVersion 1.5 | Save-Script -Path C:\\ScriptSharingDemo
Test-ScriptFileInfo C:\\ScriptSharingDemo\\Fabrikam-ClientScript.ps1

Version Name Author Description
------- ---- ------ -----------
1.5 Fabrikam-ClientScript manikb Description for the Fabrikam-ClientScript script
```


<!--HONumber=Oct16_HO1-->


