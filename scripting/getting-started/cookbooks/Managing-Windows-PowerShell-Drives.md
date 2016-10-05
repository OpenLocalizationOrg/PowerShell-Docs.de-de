---
title: Verwalten von Windows PowerShell-Laufwerken
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: bd809e38-8de9-437a-a250-f30a667d11b4
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: d266a109b1acd97c03594f988ce2fab3c697b80c

---

# Verwalten von Windows PowerShell-Laufwerken
Ein *Windows PowerShell-Laufwerk* ist ein Speicherort für Daten, auf den Sie wie auf ein Dateisystemlaufwerk in Windows PowerShell zugreifen können. Die Windows PowerShell-Anbieter erstellen einige Laufwerke für Sie, z. B. die Dateisystemlaufwerke (einschließlich C: und D:), die Registrierungslaufwerke (HKCU: und HKLM:) und das Zertifikatlaufwerk (Cert:). Sie können auch eigene Windows PowerShell-Laufwerke erstellen. Diese Laufwerke sind sehr nützlich, aber nur in Windows PowerShell verfügbar. Sie können darauf nicht mit anderen Windows-Tools, z. B. Datei-Explorer oder „Cmd.exe“, zugreifen.

Windows PowerShell verwendet das Nomen **PSDrive** für Befehle, die mit Windows PowerShell-Laufwerken arbeiten. Verwenden Sie zum Anzeigen einer Liste der Windows PowerShell-Laufwerke in Ihrer Windows PowerShell-Sitzung das Cmdlet **Get-PSDrive**.

```
PS> Get-PSDrive

Name       Provider      Root                                   CurrentLocation
----       --------      ----                                   ---------------
A          FileSystem    A:\
Alias      Alias
C          FileSystem    C:\                                 ...And Settings\me
cert       Certificate   \
D          FileSystem    D:\
Env        Environment
Function   Function
HKCU       Registry      HKEY_CURRENT_USER
HKLM       Registry      HKEY_LOCAL_MACHINE
Variable   Variable
```

Die anzeigten Laufwerke hängen von den in Ihrem System vorhandenen Laufwerken ab. Die Liste sieht jedoch ähnlich wie die oben dargestellte Ausgabe des Befehls **Get-PSDrive** aus.

Dateisystemlaufwerke sind eine Teilmenge der Windows PowerShell-Laufwerke. Sie können die Dateisystemlaufwerke am Eintrag „FileSystem“ in der Spalte „Provider“ erkennen. (Die Dateisystemlaufwerke in Windows PowerShell werden vom Windows PowerShell-Anbieter „FileSystem“ unterstützt.)

Um die Syntax des Cmdlets **Get-PSDrive** anzuzeigen, geben Sie den Befehl **Get-Command** mit dem Parameter **Syntax** ein:

```
PS> Get-Command -Name Get-PSDrive -Syntax
Get-PSDrive [[-Name] <String[]>] [-Scope <String>] [-PSProvider <String[]>] [-V
erbose] [-Debug] [-ErrorAction <ActionPreference>] [-ErrorVariable <String>] [-
OutVariable <String>] [-OutBuffer <Int32>]
```

Mit dem Parameter **PSProvider** können Sie nur die Windows PowerShell-Laufwerke anzeigen lassen, die von einem bestimmten Anbieter unterstützt werden. Um beispielsweise nur die Windows PowerShell-Laufwerke anzuzeigen, die vom Windows PowerShell-Anbieter „FileSystem“ unterstützt werden, geben Sie den Befehl **Get-PSDrive** mit dem Parameter **PSProvider** und dem Wert **FileSystem** ein:

```
PS> Get-PSDrive -PSProvider FileSystem

Name       Provider      Root                                   CurrentLocation
----       --------      ----                                   ---------------
A          FileSystem    A:\
C          FileSystem    C:\                           ...nd Settings\PowerUser
D          FileSystem    D:\
```

Um die Windows PowerShell-Laufwerke anzuzeigen, die Registrierungsstrukturen darstellen, verwenden Sie den Parameter **PSProvider**, damit nur die Windows PowerShell-Laufwerke angezeigt werden, die vom Windows PowerShell-Anbieter „Registry“ unterstützt werden:

<pre>PS> Get-PSDrive -PSProvider Registry Name       Provider      Root                                   CurrentLocation ----       --------      ----                                   --------------- HKCU       Registry      HKEY_CURRENT_USER HKLM       Registry      HKEY_LOCAL_MACHINE</pre>

Sie können auch die standardmäßigen „Location“-Cmdlets mit den Windows PowerShell-Laufwerken verwenden:

<pre>PS> Set-Location HKLM:\SOFTWARE PS> Push-Location .\Microsoft PS> Get-Location Path ---- HKLM:\SOFTWARE\Microsoft</pre>

### Hinzufügen neuer Windows PowerShell-Laufwerke (New-PSDrive)
Mit dem Befehl **New-PSDrive** können Sie eigene Windows PowerShell-Laufwerke hinzufügen. Um die Syntax des Befehls **New-PSDrive** abzurufen, geben Sie den Befehl **Get-Command** mit dem Parameter **Syntax** ein:

```
PS> Get-Command -Name New-PSDrive -Syntax
New-PSDrive [-Name] <String> [-PSProvider] <String> [-Root] <String> [-Descript
ion <String>] [-Scope <String>] [-Credential <PSCredential>] [-Verbose] [-Debug
] [-ErrorAction <ActionPreference>] [-ErrorVariable <String>] [-OutVariable <St
ring>] [-OutBuffer <Int32>] [-WhatIf] [-Confirm]
```

Um ein neues Windows PowerShell-Laufwerk zu erstellen, müssen Sie drei Parameter angeben:

-   Einen Namen für das Laufwerk (Sie können einen beliebigen gültigen Windows PowerShell-Namen verwenden)

-   PSProvider (verwenden Sie „FileSystem“ für Dateisystemspeicherorte und „Registry“ für Registrierungsspeicherorte)

-   Das Stammelement, d. h. den Pfad zum Stamm des neuen Laufwerks

Sie können beispielsweise ein Laufwerk namens „Office“ erstellen, das dem Ordner zugeordnet ist, der die Microsoft Office-Anwendungen auf Ihrem Computer enthält, z. B. **C:\Program Files\Microsoft Office\OFFICE11**. Geben Sie zum Erstellen des Laufwerks den folgenden Befehl ein:

```
PS> New-PSDrive -Name Office -PSProvider FileSystem -Root "C:\Program Files\Micr
osoft Office\OFFICE11"

Name       Provider      Root                                   CurrentLocation
----       --------      ----                                   ---------------
Office     FileSystem    C:\Program Files\Microsoft Offic...
```

> [!NOTE]
> Bei Pfaden wird im Allgemeinen Groß-/Kleinschreibung nicht beachtet.

Sie verweisen auf das neue Windows PowerShell-Laufwerk wie auf alle Windows PowerShell-Laufwerke – über den Namen gefolgt von einem Doppelpunkt (**:**).

Ein Windows PowerShell-Laufwerk kann viele Aufgaben einfacher machen. Beispielsweise haben einige der wichtigsten Schlüssel in der Windows-Registrierung äußerst lange Pfade, die den Zugriff darauf mühsam machen und schwer zu merken sind. Wichtige Konfigurationsinformationen befinden sich unter **HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion**. Zum Anzeigen und Ändern von Elementen im Registrierungsschlüssel „CurrentVersion“ können Sie ein Windows PowerShell-Laufwerk mit dem Stamm in diesem Schlüssel erstellen, indem Sie Folgendes eingeben:

<pre>PS> New-PSDrive -Name cvkey -PSProvider Registry -Root HKLM\Software\Microsoft\W indows\CurrentVersion Name       Provider      Root                                   CurrentLocation ----       --------      ----                                   --------------- cvkey      Registry      HKLM\Software\Microsoft\Windows\...</pre>

Sie können dann zum Laufwerk **cvkey:** auf dieselbe Weise wechseln, wie Sie zu jedem anderen Laufwerk wechseln:

`PS> cd cvkey:`

oder:

<pre>PS> Set-Location cvkey: -PassThru Path ---- cvkey:\</pre>

Das Cmdlet „New-PsDrive“ fügt das neue Laufwerk nur für die aktuelle Windows PowerShell-Sitzung hinzu. Wenn Sie das Windows PowerShell-Fenster schließen, geht das neue Laufwerk verloren. Wenn Sie ein Windows PowerShell-Laufwerk speichern möchten, verwenden Sie das Cmdlet „Export-Console“, um die aktuelle Windows PowerShell-Sitzung zu exportieren, und verwenden Sie dann den „PowerShell.exe“-Parameter **PSConsoleFile**, um sie zu importieren. Sie können auch das neue Laufwerk Ihrem Windows PowerShell-Profil hinzufügen.

### Löschen von Windows PowerShell-Laufwerken (Remove-PSDrive)
Über das Cmdlet **Remove-PSDrive** können Sie Laufwerke aus Windows PowerShell löschen. Das Cmdlet **Remove-PSDrive** ist einfach zu verwenden. Um ein bestimmtes Windows PowerShell-Laufwerk zu löschen, geben Sie lediglich den Namen des Windows PowerShell-Laufwerk ein.

Wenn Sie beispielsweise das Windows PowerShell-Laufwerk **Office:** hinzugefügt haben, wie im Thema **New-PSDrive** gezeigt, können Sie es löschen, indem Sie Folgendes eingeben:

```
PS> Remove-PSDrive -Name Office
```

Zum Löschen des Windows PowerShell-Laufwerks **cvkey:**, das ebenfalls im Thema **New-PSDrive** gezeigt wird, verwenden Sie den folgenden Befehl:

```
PS> Remove-PSDrive -Name cvkey
```

Das Löschen eines Windows PowerShell-Laufwerk ist einfach. Sie können es jedoch nur löschen, wenn Sie sich nicht auf dem Laufwerk befinden. Beispiel: Beispiel:

```
PS> cd office:
PS Office:\> remove-psdrive -name office
Remove-PSDrive : Cannot remove drive 'Office' because it is in use.
At line:1 char:15
+ remove-psdrive  <<<< -name office
```

### Hinzufügen und Entfernen von Laufwerken außerhalb von Windows PowerShell
Windows PowerShell erkennt Dateisystemlaufwerke, die in Windows hinzugefügt oder gelöscht werden, einschließlich Netzlaufwerken, die zugeordnet sind, USB-Laufwerken, die angeschlossen sind, und Laufwerken, die über den Befehl **net use** oder die Methoden **WScript.NetworkMapNetworkDrive** und **RemoveNetworkDrive** von einem WSH-Skript (Windows Script Host) gelöscht werden.




<!--HONumber=Oct16_HO1-->


