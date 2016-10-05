---
title: Das ISEEditor-Objekt
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 0101daf8-4e31-4e4c-ab89-01d95dcb8f46
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 020c94511ab5c0b4a19611967071e071242fadad

---

# Das ISEEditor-Objekt
  Ein **ISEEditor**-Objekt ist eine Instanz der Microsoft.PowerShell.Host.ISE.ISEEditor-Klasse. Der Konsolenbereich ist ein **ISEEditor**-Objekt. Jedes [ISEFile](The-ISEFile-Object.md)-Objekt verfügt über ein zugeordnetes **ISEEditor**-Objekt. In den folgenden Abschnitten werden die Methoden und Eigenschaften eines **ISEEditor**-Objekts aufgeführt.

## Methoden

### Clear()
  In Windows PowerShell ISE 2.0 und höher unterstützt. 

 Löscht den Text im Editor.

```PowerShell
# Clears the text in the Console pane.
$psISE.CurrentPowerShellTab.ConsolePane.Clear()
```

### EnsureVisible(int lineNumber)
  In Windows PowerShell ISE 2.0 und höher unterstützt. 

 Führt einen Bildlauf im Editor aus, sodass die Zeile, die dem angegebenen Wert des **lineNumber**-Parameters entspricht, angezeigt wird. Es wird eine Ausnahme ausgelöst, wenn die angegebene Zeilennummer nicht zwischen 1 und der Nummer der letzten Zeile liegt. Dieser Bereich definiert die gültigen Zeilennummern.

 **lineNumber**
 Die Nummer der Zeile, die angezeigt werden soll.

```PowerShell
# Scrolls the text in the Script pane so that the fifth line is in view. 
$psISE.CurrentFile.Editor.EnsureVisible(5)
```

### Focus()
  In Windows PowerShell ISE 2.0 und höher unterstützt. 

 Legt den Fokus auf den Editor fest.

```PowerShell
# Sets focus to the Console pane. 
$psISE.CurrentPowerShellTab.ConsolePane.Focus()
```

### GetLineLength(int lineNumber)
  In Windows PowerShell ISE 2.0 und höher unterstützt. 

 Ruft für die durch die Zeilennummer angegebene Zeile die Länge der Zeile als ganze Zahl ab.

 **lineNumber**
 Die Nummer der Zeile, deren Länge abgerufen werden soll.

 **Returns**
 Die Zeilenlänge für die Zeile mit der angegebenen Zeilennummer.

```PowerShell
# Gets the length of the first line in the text of the Command pane. 
$psISE.CurrentPowerShellTab.ConsolePane.GetLineLength(1)
```

### GoToMatch()
  In Windows PowerShell ISE 3.0 und höher unterstützt, in früheren Versionen nicht enthalten. 

 Verschiebt die Einfügemarke in das entsprechende Zeichen, wenn der **CanGoToMatch** -Eigenschaft des Editor-Objekts ist **$true**, der auftritt, wenn die Einfügemarke direkt vor einer öffnenden Klammer, die Klammer oder geschweiften Klammer - ist \ (, \ [, {– oder eine schließende runde Klammer, eine Klammer oder eine geschweifte Klammer unmittelbar – \), \],}.  Der Textcursor wird vor einer öffnenden bzw. nach einer schließenden Klammer platziert. Wenn die **CanGoToMatch**-Eigenschaft **$false** ist, wird mit dieser Methode keine Aktion ausgeführt. Siehe [CanGoToMatch](#cangotomatch).

```PowerShell
# Test to see if the caret is next to a parenthesis, bracket, or brace.
```

### InsertText( text )
  In Windows PowerShell ISE 2.0 und höher unterstützt. 

 Ersetzt die Markierung durch Text oder fügt Text an der aktuellen Position des Textcursors ein.

 **text**: Zeichenfolge – der einzufügende Text.

 Weitere Informationen finden Sie im [Beispielskript](#example) weiter unten in diesem Thema.

### Select( startLine, startColumn, endLine, endColumn )
  In Windows PowerShell ISE 2.0 und höher unterstützt. 

 Wählt den Text anhand der Parameter **startLine**, **startColumn**, **endLine** und **endColumn** aus.

 **startLine**: ganze Zahl – die Zeile, in der die Auswahl beginnt.

 **startColumn**: ganze Zahl – die Spalte in der Startzeile, in der die Auswahl beginnt.

 **endLine**: ganze Zahl – die Zeile, in der die Auswahl endet.

 **endColumn**: ganze Zahl – die Spalte in der Endzeile, in der die Auswahl endet.

 Weitere Informationen finden Sie im [Beispielskript](#example) weiter unten in diesem Thema.

### SelectCaretLine()
  In Windows PowerShell ISE 2.0 und höher unterstützt. 

 Wählt die gesamte Textzeile aus, in der sich der Textcursor gerade befindet.

```PowerShell
# First, set the caret position on line 5.
$psISE.CurrentFile.Editor.SetCaretPosition(5,1) 
# Now select that entire line of text
$psISE.CurrentFile.Editor.SelectCaretLine()
```

### SetCaretPosition( lineNumber, columnNumber )
  In Windows PowerShell ISE 2.0 und höher unterstützt. 

 Legt die Position des Textcursors auf die Zeilennummer und die Spaltennummer fest. Es wird eine Ausnahme ausgelöst, wenn die Zeilennummer des Textcursors oder die Spaltennummer des Textcursors außerhalb des jeweils gültigen Bereichs liegt.

 **lineNumber**: ganze Zahl – die Zeilennummer des Textcursors.

 **columnNumber**: ganze Zahl – die Spaltennummer des Textcursors.

```PowerShell
# Set the CaretPosition.
$psISE.CurrentFile.Editor.SetCaretPosition(5,1)
```

### ToggleOutliningExpansion()
  In Windows PowerShell ISE 3.0 und höher unterstützt, in früheren Versionen nicht enthalten. 

 Führt dazu, dass alle Gliederungsabschnitte erweitert oder reduziert werden.

```PowerShell
# Toggle the outlining expansion
$psISE.CurrentFile.Editor.ToggleOutliningExpansion()
```

## Eigenschaften

###  <a name="CanGoToMatch"></a> CanGoToMatch
  In Windows PowerShell ISE 3.0 und höher unterstützt, in früheren Versionen nicht enthalten. 

 Die schreibgeschützte boolesche Eigenschaft geben an, ob die Einfügemarke neben eine Klammer, die Klammer oder geschweiften Klammer: \ (\), \ [\], {}. Wenn der Textcursor sich direkt vor der öffnenden Klammer oder unmittelbar nach der schließenden Klammer eines Klammerpaars befindet, lautet der Wert dieser Eigenschaft **$true**. Andernfalls lautet er **$false**.

```PowerShell
# Test to see if the caret is next to a parenthesis, bracket, or brace
$psISE.CurrentFile.Editor.CanGoToMatch
```

###  <a name="CaretColumn"></a> CaretColumn
  In Windows PowerShell ISE 2.0 und höher unterstützt. 

 Die schreibgeschützte Eigenschaft, die die Spaltennummer abruft, die der Position des Textcursors entspricht.

```PowerShell
# Get the CaretColumn.
$psISE.CurrentFile.Editor.CaretColumn
```

###  <a name="CaretLine"></a> CaretLine
  In Windows PowerShell ISE 2.0 und höher unterstützt. 

 Die schreibgeschützte Eigenschaft, die die Nummer der Zeile mit dem Textcursor abruft.

```PowerShell
# Get the CaretLine.
$psISE.CurrentFile.Editor.CaretLine
```

###  <a name="caretlinetext"></a> CaretLineText
  In Windows PowerShell ISE 2.0 und höher unterstützt. 

 Die schreibgeschützte Eigenschaft, die die vollständige Textzeile abruft, die den Textcursor enthält.

```PowerShell
# Get all of the text on the line that contains the caret.
$psISE.CurrentFile.Editor.CaretLineText
```

###  <a name="LineCount"></a> LineCount
  In Windows PowerShell ISE 2.0 und höher unterstützt. 

 Die schreibgeschützte Eigenschaft, die die Anzahl der Zeilen aus dem Editor abruft.

```PowerShell
# Get the LineCount.
$psISE.CurrentFile.Editor.LineCount
```

###  <a name="SelectedText"></a> SelectedText
  In Windows PowerShell ISE 2.0 und höher unterstützt. 

 Die schreibgeschützte Eigenschaft, die den ausgewählten Text aus dem Editor abruft.

 Weitere Informationen finden Sie im [Beispielskript](#example) weiter unten in diesem Thema.

###  <a name="Text"></a> Text
  In Windows PowerShell ISE 2.0 und höher unterstützt. 

 Die Lese-/Schreibeigenschaft, die den Text im Editor abruft oder festlegt.

 Weitere Informationen finden Sie im [Beispielskript](#example) weiter unten in diesem Thema.

##  <a name="example"></a> Scripting-Beispiel

```PowerShell
# This illustrates how you can use the length of a line to
# select the entire line and shows how you can make it lowercase. 
# You must run this in the Console pane. It will not run in the Script pane.
# Begin by getting a variable that points to the editor.
$myEditor = $psISE.CurrentFile.Editor
# Clear the text in the current file editor.
$myEditor.Clear()

# Make sure the file has five lines of text.
$myEditor.InsertText("LINE1 `n")
$myEditor.InsertText("LINE2 `n")
$myEditor.InsertText("LINE3 `n")
$myEditor.InsertText("LINE4 `n")
$myEditor.InsertText("LINE5 `n")

# Use the GetLineLength method to get the length of the third line. 
$endColumn= $myEditor.GetLineLength(3)
# Select the text in the first three lines.
$myEditor.Select(1,1,3,$endColumn + 1)
$selection = $myEditor.SelectedText
# Clear all the text in the editor.
$myEditor.Clear()
# Add the selected text back, but in lower case.
$myEditor.InsertText($selection.ToLower())
```

## Weitere Informationen
 [Das ISEFile-Objekt](The-ISEFile-Object.md) 
 [Das PowerShellTab-Objekt](The-PowerShellTab-Object.md) 
 [Das Windows PowerShell ISE-Skriptobjektmodell](The-Windows-PowerShell-ISE-Scripting-Object-Model.md) 
 [Referenz zum Windows PowerShell ISE-Objektmodell](Windows-PowerShell-ISE-Object-Model-Reference.md) 
 [Die ISE-Objektmodellhierarchie](The-ISE-Object-Model-Hierarchy.md)

  



<!--HONumber=Oct16_HO1-->


