---
title: Das ISEOptions-Objekt
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 75e2a76f-f3d1-490b-ad5d-e3829946aabb
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 1aee849dd8b89492a641560ed4ef163c3cb96da4

---

# Das ISEOptions-Objekt
  Das **ISEOptions**-Objekt stellt verschiedene Einstellungen für Windows PowerShell ISE dar. Es ist eine Instanz der **Microsoft.PowerShell.Host.ISE.ISEOptions**-Klasse.

 Das **ISEOptions**-Objekt stellt die folgenden Methoden und Eigenschaften bereit.

 **Methoden**

-   [RestoreDefaultConsoleTokenColors()](#rdctc)

-   [RestoreDefaults()](#rd)

-   [RestoreDefaultTokenColors()](#rdtc)

-   [RestoreDefaultXmlTokenColors()](#rdxtc)

 **Eigenschaften**

-   [AutoSaveMinuteInterval](#asmi)

-   [CommandPaneBackgroundColor](#cpbc)

-   [CommandPaneUp](#cpu)

-   [ConsolePaneBackgroundColor](#conpbc)

-   [ConsolePaneForegroundColor](#conpfc)

-   [ConsolePaneTextBackgroundColor](#conptbc)

-   [ConsoleTokenColors](#contc)

-   [DebugBackgroundColor](#dbc)

-   [DebugForegroundColor](#dfc)

-   [DefaultOptions](#do)

-   [ErrorBackgroundColor](#ebc)

-   [ErrorForegroundColor](#efc)

-   [FontName](#fn)

-   [FontSize](#fs)

-   [IntellisenseTimeoutInSeconds](#itis)

-   [MruCount](#mc)

-   [OutputPaneBackgroundColor](#opbc)

-   [OutputPaneTextForegroundColor](#optfc)

-   [OutputPaneTextBackgroundColor](#optbc)

-   [ScriptPaneBackgroundColor](#spbc)

-   [ScriptPaneForegroundColor](#spfc)

-   [SelectedScriptPaneState](#ssps)

-   [ShowDefaultSnippets](#sds)

-   [ShowIntellisenseInConsolePane](#siicp)

-   [ShowIntellisenseInScriptPane](#siisp)

-   [ZeilennummernAnzeigen](#sln)

-   [ShowOutlining](#so)

-   [ShowToolBar](#stb)

-   [ShowWarningBeforeSavingOnRun](#swbsor)

-   [ShowWarningForDuplicateFiles](#swfdf)

-   [TokenColors](#tc)

-   [UseEnterToSelectInConsolePaneIntellisense](#uetsicpi)

-   [UseEnterToSelectInScriptPaneIntellisense](#uetsispi)

-   [UseLocalHelp](#ulh)

-   [VerboseBackgroundColor](#vbc)

-   [VerboseForegroundColor](#vfc)

-   [WarningBackgroundColor](#wbc)

-   [WarningForegroundColor](#wfc)

-   [XmlTokenColors](#xtc)

-   [Zoom](#z)

## Methoden

###  <a name="rdctc"></a> RestoreDefaultConsoleTokenColors\(\)
  In Windows PowerShell ISE 3.0 und höher unterstützt, in früheren Versionen nicht enthalten.

 Stellt die Standardwerte für die Tokenfarben im Konsolenbereich wieder her.

```
# Changes the color of the commands in the Console pane to red and then restores it to its default value.
$psISE.Options.ConsoleTokenColors["Command"] = "red"
$psISE.Options.RestoreDefaultConsoleTokenColors()
```

###  <a name="rd"></a> RestoreDefaults\(\)
  In Windows PowerShell ISE 2.0 und höher unterstützt.

 Stellt die Standardwerte aller Optionseinstellungen im Konsolenbereich wieder her. Außerdem wird das Verhalten von verschiedenen Warnmeldungen zurückgesetzt, in denen das Standardkontrollkästchen angezeigt wird, mit dem das erneute Anzeigen der Nachricht verhindert werden kann.

```
# Changes the background color in the Console pane and then restores it to its default value.
$psISE.Options.ConsolePaneBackgroundColor = "orange"
$psISE.Options.RestoreDefaults()
```

###  <a name="rdtc"></a> RestoreDefaultTokenColors\(\)
  In Windows PowerShell ISE 2.0 und höher unterstützt.

 Stellt die Standardwerte für die Tokenfarben im Skriptbereich wieder her.

```
# Changes the color of the comments in the Script pane to red and then restores it to its default value.
$psISE.Options.TokenColors["Comment"]="red"
$psISE.Options.RestoreDefaultTokenColors()
```

###  <a name="rdxtc"></a> RestoreDefaultXmlTokenColors\(\)
  In Windows PowerShell ISE 3.0 und höher unterstützt, in früheren Versionen nicht enthalten.

 Stellt die Standardwerte für die Tokenfarben für XML-Elemente wieder her, die in Windows PowerShell ISE angezeigt werden. Siehe auch [XmlTokenColors](#xtc).

```
# Changes the color of the comments in XML data to red and then restores it to its default value.
$psISE.Options.XmlTokenColors["Comment"]="red"
$psISE.Options.RestoreDefaultXmlTokenColors()
```

## Eigenschaften

###  <a name="asmi"></a> AutoSaveMinuteInterval
  In Windows PowerShell ISE 3.0 und höher unterstützt, in früheren Versionen nicht enthalten.

 Gibt die Anzahl der Minuten zwischen automatischen Speichervorgängen Ihrer Dateien durch Windows PowerShell ISE an. Der Standardwert beträgt 2 Minuten. Der Wert ist eine ganze Zahl.

```
# Changes the number of minutes between automatic save operations to every 3 minutes.
$psISE.Options.AutoSaveMinuteInterval = 3
```

###  <a name="cpbc"></a> CommandPaneBackgroundColor
  Dieses Feature ist in Windows PowerShell ISE 2.0 enthalten, wurde in höheren Versionen von ISE aber entfernt oder umbenannt.  Informationen zu höheren Versionen finden Sie unter [ConsolePaneBackgroundColor](#conpbc).

 Gibt die Hintergrundfarbe für den Befehlsbereich an. Dies ist eine Instanz der **System.Windows.Media.Color**-Klasse.

```
# Changes the background color of the Command pane to orange.
$psISE.Options.CommandPaneBackgroundColor = "orange"
```

###  <a name="cpu"></a> CommandPaneUp
  Dieses Feature ist in Windows PowerShell ISE 2.0 enthalten, wurde in höheren Versionen von ISE aber entfernt oder umbenannt.

 Gibt an, ob sich der Befehlsbereich oberhalb des Ausgabebereichs befindet.

```
# Moves the Command pane to the top of the screen.
$psISE.Options.CommandPaneUp  = $true

```

###  <a name="conpbc"></a> ConsolePaneBackgroundColor
  In Windows PowerShell ISE 3.0 und höher unterstützt, in früheren Versionen nicht enthalten.

 Gibt die Hintergrundfarbe für den Konsolenbereich an. Dies ist eine Instanz der **System.Windows.Media.Color**-Klasse.

```
# Changes the background color of the Console pane to red.
$psISE.Options.ConsolePaneBackgroundColor = "red"
```

###  <a name="conpfc"></a> ConsolePaneForegroundColor
  In Windows PowerShell ISE 3.0 und höher unterstützt, in früheren Versionen nicht enthalten.

 Gibt die Vordergrundfarbe des Texts im Konsolenbereich an.

```
# Changes the foreground color of the text in the Console pane to yellow.
$psISE.Options.ConsolePaneForegroundColor  = "yellow"

```

###  <a name="conptbc"></a> ConsolePaneTextBackgroundColor
  In Windows PowerShell ISE 3.0 und höher unterstützt, in früheren Versionen nicht enthalten.

 Gibt die Hintergrundfarbe des Texts im Konsolenbereich an.

```
# Changes the background color of the Console pane text to pink.
$psISE.Options.ConsolePaneTextBackgroundColor = "pink"
```

###  <a name="contc"></a> ConsoleTokenColors
  In Windows PowerShell ISE 3.0 und höher unterstützt, in früheren Versionen nicht enthalten.

 Gibt die Farben der IntelliSense-Token im Windows PowerShell ISE-Konsolenbereich an. Diese Eigenschaft ist ein Wörterbuchobjekt, das Name-Wert-Paare von Tokentypen und Farben für den Konsolenbereich enthält. Informationen zum Ändern der Farben der IntelliSense-Token im Skriptbereich finden Sie unter [TokenColors](#tc). Informationen zum Zurücksetzen der Farben auf die Standardwerte finden Sie unter [RestoreDefaultConsoleTokenColors()](#rdctc). Tokenfarben können für folgende Elemente festgelegt werden: Attribute, Command, CommandArgument, CommandParameter, Comment, GroupEnd, GroupStart, Keyword, LineContinuation, LoopLabel, Member, NewLine, Number, Operator, Position, StatementSeparator, String, Type, Unknown, Variable.

```
# Sets the color of commands to green.
$psISE.Options.ConsoleTokenColors["Command"] = "green"
# Sets the color of keywords to magenta.
$psISE.Options.ConsoleTokenColors["Keyword"] = "magenta"

```

###  <a name="dbc"></a> DebugBackgroundColor
  In Windows PowerShell ISE 2.0 und höher unterstützt.

 Gibt die Hintergrundfarbe für den Debugtext an, der im Konsolenbereich angezeigt wird. Dies ist eine Instanz der **System.Windows.Media.Color**-Klasse.

```
# Changes the background color for the debug text that appears in the Console pane to blue.
$psISE.Options.DebugBackgroundColor ='#0000FF'
```

###  <a name="dfc"></a> DebugForegroundColor
  In Windows PowerShell ISE 2.0 und höher unterstützt.

 Gibt die Vordergrundfarbe für den Debugtext an, der im Konsolenbereich angezeigt wird. Dies ist eine Instanz der **System.Windows.Media.Color**-Klasse.

```
# Changes the foreground color for the debug text that appears in the Console pane to yellow.
$psISE.Options.DebugForegroundColor ="yellow"
```

###  <a name="do"></a> DefaultOptions
  In Windows PowerShell ISE 2.0 und höher unterstützt.

 Eine Sammlung von Eigenschaften, mit denen die Standardwerte angegeben werden, die bei Verwendung der Reset-Methoden verwendet werden sollen.

```
# Displays the name of the default options. This example is from ISE 4.0.
$psISE.Options.DefaultOptions

SelectedScriptPaneState                   : Top
ShowDefaultSnippets                       : True
ShowToolBar                               : True
ShowOutlining                             : True
ShowLineNumbers                           : True
TokenColors                               : {[Attribute, #FF00BFFF], [Command, #FF0000FF], [CommandArgument, #FF8A2BE2], [CommandParameter, #FF000080]...}
ConsoleTokenColors                        : {[Attribute, #FFB0C4DE], [Command, #FFE0FFFF], [CommandArgument, #FFEE82EE], [CommandParameter, #FFFFE4B5]...}
XmlTokenColors                            : {[Comment, #FF006400], [CommentDelimiter, #FF008000], [ElementName, #FF8B0000], [MarkupExtension, #FFFF8C00]...}
DefaultOptions                            : Microsoft.PowerShell.Host.ISE.ISEOptions
FontSize                                  : 9
Zoom                                      : 100
FontName                                  : Lucida Console
ErrorForegroundColor                      : #FFFF0000
ErrorBackgroundColor                      : #00FFFFFF
WarningForegroundColor                    : #FFFF8C00
WarningBackgroundColor                    : #00FFFFFF
VerboseForegroundColor                    : #FF00FFFF
VerboseBackgroundColor                    : #00FFFFFF
DebugForegroundColor                      : #FF00FFFF
DebugBackgroundColor                      : #00FFFFFF
ConsolePaneBackgroundColor                : #FF012456
ConsolePaneTextBackgroundColor            : #FF012456
ConsolePaneForegroundColor                : #FFF5F5F5
ScriptPaneBackgroundColor                 : #FFFFFFFF
ScriptPaneForegroundColor                 : #FF000000
ShowWarningForDuplicateFiles              : True
ShowWarningBeforeSavingOnRun              : True
UseLocalHelp                              : True
AutoSaveMinuteInterval                    : 2
MruCount                                  : 10
ShowIntellisenseInConsolePane             : True
ShowIntellisenseInScriptPane              : True
UseEnterToSelectInConsolePaneIntellisense : True
UseEnterToSelectInScriptPaneIntellisense  : True
IntellisenseTimeoutInSeconds              : 3

```

###  <a name="ebc"></a> ErrorBackgroundColor
  In Windows PowerShell ISE 2.0 und höher unterstützt.

 Gibt die Hintergrundfarbe für den Fehlertext an, der im Konsolenbereich angezeigt wird. Dies ist eine Instanz der **System.Windows.Media.Color**-Klasse.

```
# Changes the background color for the error text that appears in the Console pane to black.
$psISE.Options.ErrorBackgroundColor="black"
```

###  <a name="efc"></a> ErrorForegroundColor
  In Windows PowerShell ISE 2.0 und höher unterstützt.

 Gibt die Vordergrundfarbe für den Fehlertext an, der im Konsolenbereich angezeigt wird. Dies ist eine Instanz der **System.Windows.Media.Color**-Klasse.

```
# Changes the foreground color for the error text that appears in the console pane to green.
$psISE.Options.ErrorForegroundColor ="green"
```

###  <a name="fn"></a> FontName
  In Windows PowerShell ISE 2.0 und höher unterstützt.

 Gibt den Namen der Schriftart an, die derzeit im Skriptbereich und im Konsolenbereich verwendet wird.

```
# Changes the font used in both panes.
$psISE.Options.FontName = "courier new"
```

###  <a name="fs"></a> FontSize
  In Windows PowerShell ISE 2.0 und höher unterstützt.

 Gibt den Schriftgrad als ganze Zahl an. Dieser wird im Skriptbereich, Befehlsbereich und Ausgabebereich verwendet. Der gültige Wertebereich liegt zwischen 8 und 32.

```
# Changes the font size in all panes.
$psISE.Options.FontSize = 20

```

###  <a name="itis"></a> IntellisenseTimeoutInSeconds
  In Windows PowerShell ISE 3.0 und höher unterstützt, in früheren Versionen nicht enthalten.

 Gibt in Sekunden an, wie lange IntelliSense versucht, den gerade eingegebenen Text aufzulösen. Nach dieser Anzahl von Sekunden tritt in IntelliSense eine Zeitüberschreitung auf, und Sie können mit der Eingabe fortfahren. Der Standardwert sind 3 Sekunden. Der Wert ist eine ganze Zahl.

```
# Changes the number of seconds for IntelliSense syntax recognition to 5.
$psISE.Options.IntellisenseTimeoutInSeconds = 5
```

###  <a name="mc"></a> MruCount
  In Windows PowerShell ISE 3.0 und höher unterstützt, in früheren Versionen nicht enthalten.

 Gibt die Anzahl der zuletzt geöffneten Dateien an, die Windows PowerShell ISE nachverfolgt und am unteren Rand des Menüs **Datei öffnen** anzeigt. Der Standardwert ist 10. Der Wert ist eine ganze Zahl.

```
# Changes the number of recently used files that appear at the bottom of the File Open menu to 5.
$psISE.Options.MruCount = 5
```

###  <a name="opbc"></a> OutputPaneBackgroundColor
  Dieses Feature ist in Windows PowerShell ISE 2.0 enthalten, wurde in höheren Versionen von ISE aber entfernt oder umbenannt.  Informationen zu höheren Versionen finden Sie unter [ConsolePaneBackgroundColor](#conpbc).

 Die Lese-/Schreibeigenschaft, die die Hintergrundfarbe für den Ausgabebereich selbst abruft oder festlegt. Dies ist eine Instanz der **System.Windows.Media.Color**-Klasse.

```
# Changes the background color of the Output pane to gold.
$psISE.Options.OutputPaneForegroundColor = "gold"

```

###  <a name="optfc"></a> OutputPaneTextForegroundColor
  Dieses Feature ist in Windows PowerShell ISE 2.0 enthalten, wurde in höheren Versionen von ISE aber entfernt oder umbenannt.  Informationen zu höheren Versionen finden Sie unter [ConsolePaneForegroundColor](#conpfc).

 Die Lese-/Schreibeigenschaft, mit der die Vordergrundfarbe des Texts im Ausgabereich in Windows PowerShell ISE 2.0 geändert wird.

```
# Changes the foreground color of the text in the Output Pane to blue.
$psISE.Options.OutputPaneTextForegroundColor  = "blue"

```

###  <a name="optbc"></a> OutputPaneTextBackgroundColor
  Dieses Feature ist in Windows PowerShell ISE 2.0 enthalten, wurde in höheren Versionen von ISE aber entfernt oder umbenannt.  Informationen zu höheren Versionen finden Sie unter [ConsolePaneTextBackgroundColor](#conptbc).

 Die Lese-/Schreibeigenschaft, mit der die Hintergrundfarbe des Texts im Ausgabereich geändert wird.

```
# Changes the background color of the Output pane text to pink.
$psISE.Options.OutputPaneTextBackgroundColor = "pink"
```

###  <a name="spbc"></a> ScriptPaneBackgroundColor
  In Windows PowerShell ISE 2.0 und höher unterstützt.

 Die Lese-/Schreibeigenschaft, mit der die Hintergrundfarbe für Dateien abgerufen oder festgelegt wird. Dies ist eine Instanz der **System.Windows.Media.Color**-Klasse.

```

# Sets the color of the script pane background to yellow.
$psISE.Options.ScriptPaneBackgroundColor = ”yellow”

```

###  <a name="spfc"></a> ScriptPaneForegroundColor
  In Windows PowerShell ISE 2.0 und höher unterstützt.

 Die Lese-/Schreibeigenschaft, mit der die Vordergrundfarbe für Nicht-Skriptdateien im Skriptbereich abgerufen oder festgelegt wird.
Verwenden Sie zum Festlegen der Vordergrundfarbe für Skriptdateien die [TokenColors](The-ISEOptions-Object.md#tc)-Eigenschaft.

```
# Sets the foreground to color of non-script files in the script pane to green.
$psISE.Options.ScriptPaneBackgroundColor = "green"

```

###  <a name="ssps"></a> SelectedScriptPaneState
  In Windows PowerShell ISE 2.0 und höher unterstützt.

 Die Lese-/Schreibeigenschaft, die die Position des Skriptbereichs in der Anzeige abruft oder festlegt. Die Zeichenfolge kann „Maximized“, „Top“ oder „Right“ lauten.

```
# Moves the Script Pane to the top.
$psISE.Options.SelectedScriptPaneState = "Top"
# Moves the Script Pane to the right.
$psISE.Options.SelectedScriptPaneState = "Right"
# Maximizes the Script Pane
$psISE.Options.SelectedScriptPaneState = "Maximized"

```

###  <a name="sds"></a> ShowDefaultSnippets
  In Windows PowerShell ISE 3.0 und höher unterstützt, in früheren Versionen nicht enthalten.

 Gibt an, ob die **STRG+J**-Codeausschnittliste den in Windows PowerShell enthaltenen Startersatz umfasst. Bei **$false** werden nur benutzerdefinierte Codeausschnitte in der **STRG+J**-Liste angezeigt. Der Standardwert ist **$true**.

```
# Hide the default snippets from the CTRL+J list.
$psISe.Options.ShowDefaultSnippets = $false
```

###  <a name="siicp"></a> ShowIntellisenseInConsolePane
  In Windows PowerShell ISE 3.0 und höher unterstützt, in früheren Versionen nicht enthalten.

 Gibt an, ob IntelliSense Syntax-, Parameter- und Wertvorschläge im Konsolenbereich anzeigt. Der Standardwert ist **$true**.

```
# Turn off IntelliSense in the console pane.
$psISe.Options.ShowIntellisenseInConsolePane = $false
```

###  <a name="siisp"></a> ShowIntellisenseInScriptPane
  In Windows PowerShell ISE 3.0 und höher unterstützt, in früheren Versionen nicht enthalten.

 Gibt an, ob IntelliSense Syntax-, Parameter- und Wertvorschläge im Skriptbereich anzeigt. Der Standardwert ist **$true**.

```
# Turn off IntelliSense in the Script pane.
$psISe.Options.ShowIntellisenseInScriptPane = $false
```

###  <a name="sln"></a> ZeilennummernAnzeigen
  In Windows PowerShell ISE 3.0 und höher unterstützt, in früheren Versionen nicht enthalten.

 Gibt an, ob im Skriptbereich Zeilennummern am linken Rand angezeigt werden. Der Standardwert ist **$true**.

```
# Turn off line numbers in the Script pane.
$psISe.Options.ShowLineNumbers = $false
```

###  <a name="so"></a> ShowOutlining
  In Windows PowerShell ISE 3.0 und höher unterstützt, in früheren Versionen nicht enthalten.

 Gibt an, ob im Skriptbereich erweiterbare und reduzierbare Klammern neben Codeabschnitten am linken Rand angezeigt werden. Wenn sie angezeigt werden, können Sie auf das Minuszeichen klicken \(-\) Symbole neben einem Textblock zu reduzieren, oder klicken Sie auf das Plussymbol \(+\), um einen Textblock zu erweitern. Der Standardwert ist **$true**.

```
# Turn off outlining in the Script pane.
$psISe.Options.ShowOutlining = $false
```

###  <a name="stb"></a> ShowToolBar
  In Windows PowerShell ISE 2.0 und höher unterstützt.

 Gibt an, ob die ISE-Symbolleiste am oberen Rand des Windows PowerShell ISE-Fensters angezeigt wird. Der Standardwert ist **$true**.

```
# Show the toolbar.
$psISe.Options.ShowToolBar = $true
```

###  <a name="swbsor"></a> ShowWarningBeforeSavingOnRun
  In Windows PowerShell ISE 2.0 und höher unterstützt.

 Gibt an, ob eine Warnung angezeigt wird, wenn ein Skript vor dem Ausführen automatisch gespeichert wird. Der Standardwert ist **$true**.

```
# Enable the warning message when an attempt
# is made to run a script without saving it first.
$psISE.Options.ShowWarningBeforeSavingOnRun=$true

```

###  <a name="swfdf"></a> ShowWarningForDuplicateFiles
  In Windows PowerShell ISE 2.0 und höher unterstützt.

 Gibt an, ob eine Warnung angezeigt wird, wenn die gleiche Datei auf mehreren PowerShell-Registerkarten geöffnet wird. Wenn dies auf **$true** festgelegt ist und eine Datei auf mehreren Registerkarten angezeigt werden soll, wird die folgende Meldung angezeigt: „Eine Kopie dieser Datei ist in einer anderen PowerShell-Registerkarte geöffnet. Änderungen an dieser Datei betreffen alle geöffneten Kopien.“ Der Standardwert ist **$true**.

```
# Enable the warning message when a file is
# opened in multiple PowerShell tabs.
$psISE.Options.ShowWarningForDuplicateFiles = $true

```

###  <a name="tc"></a> TokenColors
  In Windows PowerShell ISE 2.0 und höher unterstützt.

 Gibt die Farben der IntelliSense-Token im Windows PowerShell ISE-Skriptbereich an. Diese Eigenschaft ist ein Wörterbuchobjekt, das Name-Wert-Paare von Tokentypen und Farben für den Skriptbereich enthält. Informationen zum Ändern der Farben der IntelliSense-Token im Konsolenbereich finden Sie unter [ConsoleTokenColors](#contc). Informationen zum Zurücksetzen der Farben auf die Standardwerte finden Sie unter [RestoreDefaultTokenColors()](#rdtc). Tokenfarben können für folgende Elemente festgelegt werden: Attribute, Command, CommandArgument, CommandParameter, Comment, GroupEnd, GroupStart, Keyword, LineContinuation, LoopLabel, Member, NewLine, Number, Operator, Position, StatementSeparator, String, Type, Unknown, Variable.

```
# Sets the color of commands to green.
$psISE.Options.TokenColors["Command"] = "green"
# Sets the color of keywords to magenta.
$psISE.Options.TokenColors["Keyword"] = "magenta"

```

###  <a name="uetsicpi"></a> UseEnterToSelectInConsolePaneIntellisense
  In Windows PowerShell ISE 3.0 und höher unterstützt, in früheren Versionen nicht enthalten.

 Gibt an, ob Sie mit der Eingabetaste eine von IntelliSense bereitgestellte Option im Konsolenbereich auswählen können. Der Standardwert ist **$true**.

```
# Turn off using the ENTER key to select an IntelliSense provided option in the Console pane.
$psISE.Options.UseEnterToSelectInConsolePaneIntellisense=$false

```

###  <a name="uetsispi"></a> UseEnterToSelectInScriptPaneIntellisense
  In Windows PowerShell ISE 3.0 und höher unterstützt, in früheren Versionen nicht enthalten.

 Gibt an, ob Sie mit der Eingabetaste eine von IntelliSense bereitgestellte Option im Skriptbereich auswählen können. Der Standardwert ist **$true**.

```
# Turn on using the Enter key to select an IntelliSense provided option in the Console pane.
$psISE.Options.UseEnterToSelectInConsolePaneIntellisense=$true

```

###  <a name="ulh"></a> UseLocalHelp
  In Windows PowerShell ISE 3.0 und höher unterstützt, in früheren Versionen nicht enthalten.

 Gibt an, ob die lokal installierte Hilfe oder die Onlinehilfe der TechNet-Bibliothek angezeigt wird, wenn der Cursor auf einem Schlüsselwort positioniert ist und Sie F1 drücken. Wenn dies auf **$true** festgelegt ist, werden in einem Popupfenster Inhalte aus der lokal installierten Hilfe angezeigt. Sie können die Hilfedateien mit dem Befehl `Update-Help` installieren. Wenn dies auf **$false** festgelegt ist, wird eine Seite in der TechNet-Bibliothek im Browser geöffnet.

```
# Sets the option for the online help to be displayed.
$psISE.Options.UseLocalHelp=$false
# Sets the option for the local Help to be displayed.
$psISE.Options.UseLocalHelp=$true

```

###  <a name="vbc"></a> VerboseBackgroundColor
  In Windows PowerShell ISE 2.0 und höher unterstützt.

 Gibt die Hintergrundfarbe für ausführlichen Text an, der im Konsolenbereich angezeigt wird. Dies ist ein **System.Windows.Media.Color**-Objekt.

```
# Changes the background color for verbose text to blue.
$psISE.Options.VerboseBackgroundColor ='#0000FF'
```

###  <a name="vfc"></a> VerboseForegroundColor
  In Windows PowerShell ISE 2.0 und höher unterstützt.

 Gibt die Vordergrundfarbe für ausführlichen Text an, der im Konsolenbereich angezeigt wird. Dies ist ein **System.Windows.Media.Color**-Objekt.

```
# Changes the foreground color for verbose text to yellow.
$psISE.Options.VerboseForegroundColor =”yellow”
```

###  <a name="wbc"></a> WarningBackgroundColor
  In Windows PowerShell ISE 2.0 und höher unterstützt.

 Gibt die Hintergrundfarbe für Warntext an, der im Konsolenbereich angezeigt wird. Dies ist ein **System.Windows.Media.Color**-Objekt.

```
# Changes the background color for warning text to blue.
$psISE.Options.WarningBackgroundColor ='#0000FF'
```

###  <a name="wfc"></a> WarningForegroundColor
  In Windows PowerShell ISE 2.0 und höher unterstützt.

 Gibt die Vordergrundfarbe für Warntext an, der im Ausgabebereich angezeigt wird. Dies ist ein **System.Windows.Media.Color**-Objekt.

```
# Changes the foreground color for warning text to yellow.
$psISE.Options.WarningForegroundColor =”yellow”
```

###  <a name="xtc"></a> XmlTokenColors
  In Windows PowerShell ISE 3.0 und höher unterstützt, in früheren Versionen nicht enthalten.

 Gibt ein Wörterbuchobjekt an, das Name-Wert-Paare von Tokentypen und Farben für XML-Inhalte enthält, die in Windows PowerShell ISE angezeigt werden. Tokenfarben können für folgende Elemente festgelegt werden: Attribute, Command, CommandArgument, CommandParameter, Comment, GroupEnd, GroupStart, Keyword, LineContinuation, LoopLabel, Member, NewLine, Number, Operator, Position, StatementSeparator, String, Type, Unknown, Variable. Siehe auch [RestoreDefaultXmlTokenColors()](#rdxtc).

```
# Sets the color of XML element names to green.
$psISE.Options.XmlTokenColors["ElementName"] = "green"
# Sets the color of XML comments to magenta.
$psISE.Options.XmlTokenColors["Comment"] = "magenta"

```

###  <a name="z"></a> Zoom
  In Windows PowerShell ISE 3.0 und höher unterstützt, in früheren Versionen nicht enthalten.

 Gibt die relative Größe des Texts im Konsolenbereich und Skriptbereich an. Der Standardwert ist 100. Bei kleineren Werten wird der Text in Windows PowerShell ISE kleiner angezeigt, bei größeren Werten wird er größer angezeigt. Der Wert ist eine ganze Zahl zwischen 20 und 400.

```
# Changes the text in the Windows PowerShell ISE to be double its normal size.
$psISE.Options.Zoom = 200
```

## Weitere Informationen
 [Das Windows PowerShell ISE-Skriptobjektmodell](The-Windows-PowerShell-ISE-Scripting-Object-Model.md)
 [Referenz zum Windows PowerShell ISE-Objektmodell](Windows-PowerShell-ISE-Object-Model-Reference.md)




<!--HONumber=Oct16_HO1-->


