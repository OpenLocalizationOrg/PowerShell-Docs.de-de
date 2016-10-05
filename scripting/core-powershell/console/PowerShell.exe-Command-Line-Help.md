---
title: "Befehlszeilenhilfe für „PowerShell.exe“"
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 1ab7b93b-6785-42c6-a1c9-35ff686a958f
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: c3b263110a908c28569cf3048a94d48da8316684

---

# Befehlszeilenhilfe für „PowerShell.exe“
Startet eine Windows PowerShell-Sitzung. Sie können mithilfe von „PowerShell.exe“ eine Windows PowerShell-Sitzung über die Befehlszeile eines anderen Tools wie z. B. „Cmd.exe“ starten oder mit diesem Befehl über die Windows PowerShell-Befehlszeile eine neue Sitzung starten. Verwenden Sie die Parameter, um die Sitzung anzupassen.

## Syntax

```
PowerShell[.exe]
       [-EncodedCommand <Base64EncodedCommand>]
       [-ExecutionPolicy <ExecutionPolicy>]
       [-InputFormat {Text | XML}] 
       [-Mta]
       [-NoExit]
       [-NoLogo]
       [-NonInteractive] 
       [-NoProfile] 
       [-OutputFormat {Text | XML}] 
       [-PSConsoleFile <FilePath> | -Version <Windows PowerShell version>]
       [-Sta]
       [-WindowStyle <style>]
       [-File <FilePath> [<Args>]]
       [-Command { - | <script-block> [-args <arg-array>]
                     | <string> [<CommandParameters>] } ]

PowerShell[.exe] -Help | -? | /?
```

## Parameter

### -EncodedCommand <Base64EncodedCommand>
Akzeptiert eine „base-64“-codierte Zeichenfolgenversion eines Befehls. Verwenden Sie diesen Parameter, um Befehle an Windows PowerShell zu übermitteln, die komplexe Anführungszeichen oder geschweifte Klammern erfordern.

### -ExecutionPolicy <ExecutionPolicy>
Legt die Standardausführungsrichtlinie für die aktuelle Sitzung fest und speichert sie in der Umgebungsvariablen „$env:PSExecutionPolicyPreference“. Dieser Parameter ändert nicht die Windows PowerShell-Ausführungsrichtlinie, die in der Registrierung festgelegt ist. Weitere Informationen zu Windows PowerShell-Ausführungsrichtlinien (einschließlich einer Liste gültiger Werte) finden Sie unter „about_Execution_Policies“ (http://go.microsoft.com/fwlink/?LinkID=135170).

### -File <FilePath> \[<Parameters>]
Führt das angegebene Skript im lokalen Bereich aus, sodass die Funktionen und Variablen, die das Skript erstellt, in der aktuellen Sitzung verfügbar sind. Geben Sie den Pfad der Skriptdatei und Parameter an. **File** muss der letzte Parameter im Befehl sein, da alle Zeichen, die nach dem **File**-Parameter eingegeben werden, als Pfad der Skriptdatei gefolgt von den Skriptparametern und ihren Werten interpretiert werden.

Sie können in den Wert des **File**-Parameters die Parameter eines Skripts und Parameterwerte einschließen. Beispiel: `-File .\Get-Script.ps1 -Domain Central`

Die Switch-Parameter eines Skripts werden in der Regel entweder einbezogen oder ausgelassen. Beispielsweise der folgende Befehl verwendet die **alle** Parameter der Skriptdatei Get-Script.ps1: `-File .\Get-Script.ps1 -All`

In seltenen Fällen müssen Sie für einen Switch-Parameter einen booleschen Wert angeben. Zum Angeben eines booleschen Werts für einen Switch-Parameter im Wert des **File**-Parameters setzen Sie den Namen und Wert des Parameters in geschweifte Klammern. Siehe das folgende Beispiel: `-File .\Get-Script.ps1 {-All:$False}`

### -InputFormat {Text | XML}
Beschreibt das Format der Daten, die an Windows PowerShell übermittelt werden. Gültige Werte sind "Text" (Textzeichenfolgen) oder "XML" (serialisiertes CLIXML-Format).

### -Mta
Startet Windows PowerShell mit einem Multithread-Apartment. Dieser Parameter wird in Windows PowerShell 3.0 eingeführt. In Windows PowerShell 3.0 ist Singlethread-Apartment (STA) die Standardeinstellung. In Windows PowerShell 2.0 ist Multithread-Apartment (MTA) die Standardeinstellung.

### -NoExit
Nach dem Ausführen der Startbefehle erfolgt kein Beenden.

### -NoLogo
Blendet die Copyrightinformationen beim Start aus.

### -NonInteractive
Zeigt dem Benutzer keine interaktive Eingabeaufforderung.

### -NoProfile
Das Windows PowerShell-Profil wird nicht geladen.

### -OutputFormat {Text | XML}
Bestimmt, wie die Ausgabe von Windows PowerShell formatiert ist. Gültige Werte sind "Text" (Textzeichenfolgen) oder "XML" (serialisiertes CLIXML-Format).

### -PSConsoleFile <FilePath>
Lädt die angegebene Windows PowerShell-Konsolendatei. Geben Sie den Pfad und Namen der Konsolendatei ein. Verwenden Sie zum Erstellen einer Konsolendatei das Cmdlet [Export-Console](https://technet.microsoft.com/en-us/library/4bab1c02-9e61-4aaf-9957-11d1934ef4ef) in Windows PowerShell.

### -Sta
Startet Windows PowerShell mit einem Singlethread-Apartment. In Windows PowerShell 3.0 ist Singlethread-Apartment (STA) die Standardeinstellung. In Windows PowerShell 2.0 ist Multithread-Apartment (MTA) die Standardeinstellung.

### -Version <Windows PowerShell Version>
Startet die angegebene Version von Windows PowerShell. Die Version, die Sie angeben, muss auf dem System installiert sein. Falls Windows PowerShell 3.0 auf dem Computer installiert ist, sind „3.0“ und „2.0“ gültige Werte. Der Standardwert ist „3.0“.

Falls Windows PowerShell 3.0 nicht installiert ist, ist „2.0“ der einzige gültige Wert. Andere Werte werden ignoriert.

Weitere Informationen finden Sie unter „Installieren von Windows PowerShell“ in [Erste Schritte mit Windows PowerShell [ALTES MSDN]](https://technet.microsoft.com/en-us/library/69555d95-b481-43e1-86e7-b46d68b3e2dd).

### WindowStyle- <Window style>
Legt den Fensterstil für die Sitzung fest. Gültige Werte sind „Normal“, „Minimized“, „Maximized“ und „Hidden“.

### -Command
Führt die angegebenen Befehle (samt Parametern) so aus, als wären über die Windows PowerShell-Befehlszeile eingegeben worden, und wird dann beendet, es sei denn, der „NoExit“-Parameter wird angegeben.

Der Wert von „Command“ kann „-“, eine Zeichenfolge oder ein Skriptblock sein. Wenn der Wert des Befehls „-“ ist, wird der Befehlstext aus der Standardeingabe gelesen.

Skriptblöcke müssen in geschweifte Klammern ({}) gesetzt werden. Sie können einen Skriptblock nur angeben, wenn „PowerShell.exe“ in Windows PowerShell ausgeführt wird. Die Ergebnisse des Skripts werden an die übergeordnete Shell als deserialisierte XML-Objekte und nicht als Live-Objekte zurückgegeben.

Wenn der Wert von „Command“ eine Zeichenfolge ist, muss **Command** der letzte Parameter im Befehl sein, da alle Zeichen, die nach dem Befehl eingegeben werden, als die Befehlsargumente interpretiert werden.

Um eine Zeichenfolge zu schreiben, die einen Windows PowerShell-Befehl ausführt, verwenden Sie das Format:

```
"& {<command>}"
```

wobei die Anführungszeichen eine Zeichenfolge angeben und der Aufrufoperator (&) bewirkt, dass der Befehl ausgeführt wird.

### -Help, -?, /?
Zeigt diese Meldung. Wenn Sie einen „PowerShell.exe“-Befehl in Windows PowerShell eingeben, setzen Sie vor die Befehlsparameter einen Bindestrich (-) und keinen Schrägstrich (/). In „Cmd.exe“ können Sie entweder einen Binde- oder Schrägstrich verwenden.

> [!NOTE]
> Hinweis zur Fehlerbehebung: In Windows PowerShell 2.0 misslingt das Starten einiger Programme in der Windows PowerShell-Konsole mit dem folgenden „LastExitCode“: 0xc0000142.

## BEISPIELE

```
PowerShell -PSConsoleFile sqlsnapin.psc1

PowerShell -Version 2.0 -NoLogo -InputFormat text -OutputFormat XML

PowerShell -Command "Get-EventLog -LogName security"

# in an existing PowerShell session that understands the curly braces mean a script block
PowerShell -Command {Get-EventLog -LogName security}

PowerShell -Command "& {Get-EventLog -LogName security}"

# To use the -EncodedCommand parameter:
$command = "dir 'c:\program files' "
$bytes = [System.Text.Encoding]::Unicode.GetBytes($command)
$encodedCommand = [Convert]::ToBase64String($bytes)
powershell.exe -encodedCommand $encodedCommand
```




<!--HONumber=Oct16_HO1-->


