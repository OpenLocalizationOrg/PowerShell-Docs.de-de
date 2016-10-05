---
title: 'So wird''s gemacht: Debuggen von Skripts in Windows PowerShell ISE'
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 6dc6d8f9-8978-46e9-a92f-169af37e2817
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 30cc1671518fe5f134556443dca7ae46e9be3fea

---

# So wird's gemacht: Debuggen von Skripts in Windows PowerShell ISE
In diesem Thema wird beschrieben, wie das Debuggen von Skripts auf einem lokalen Computer mithilfe der visuellen Debugfunktionen von Windows PowerShell® Integrated Scripting Environment (ISE) erfolgt.

[Verwalten von Haltepunkten](#bkmk_1)
[Verwalten einer Debugsitzung](#bkmk_2)
[Schrittweises Debuggen: Überspringen, Einzelschritt und Rücksprung](#bkmk_3)
[Anzeigen der Werte von Variablen beim Debuggen](#bkmk_4)

## <a name="bkmk_1"></a>Verwalten von Haltepunkten
Ein Haltepunkt ist eine bestimmte Stelle in einem Skript, an der die Verarbeitung angehalten werden soll, damit Sie den aktuellen Status der Variablen sowie die Umgebung prüfen können, in der das Skript ausgeführt wird. Sobald Ihr Skript an einem Haltepunkt angehalten wird, können Sie Befehle im Konsolenbereich ausführen, um den Status des Skripts zu prüfen.  Sie können Variablen ausgeben oder andere Befehle ausführen. Sie können sogar die Werte aller Variablen ändern, die im Kontext des Skripts sichtbar sind, das derzeit ausgeführt wird. Nachdem Sie die Elemente geprüft haben, die Sie sich ansehen wollten, können Sie das Ausführen des Skripts fortsetzen.

In der Windows PowerShell-Debugumgebung können Sie drei Typen von Haltepunkten festlegen:

1.  **Zeilenhaltepunkt**. Das Skript wird angehalten, wenn die bestimmte Zeile beim Ausführen des Skripts erreicht wird.

2.  **Variable Haltepunkt.** Das Skript wird immer dann angehalten, wenn sich der Wert der bestimmten Variablen ändert.

3.  **Befehl Haltepunkt.** Das Skript wird immer dann angehalten, wenn der bestimmte Befehl beim Ausführen des Skripts der nächste auszuführende Befehl ist. Der Haltepunkt kann Parameter enthalten, um ihn weiter entsprechend dem von Ihnen gewünschten Vorgang zu filtern. Der Befehl kann auch eine Funktion sein, die Sie erstellt haben.

Für diese Haltepunkte ist zu beachten, dass in der Windows PowerShell ISE-Debugumgebung nur Zeilenhaltepunkte über das Menü oder über Tastenkombinationen festgelegt werden können. Die beiden anderen Typen von Haltepunkten können festgelegt werden, dies erfolgt jedoch aus dem Konsolenbereich mit dem Cmdlet [Set-PSBreakpoint](https://technet.microsoft.com/library/88d2d9ad-17dc-44ae-99aa-f841125b9dc8). In diesem Abschnitt wird beschrieben, wie Sie Debugaufgaben in Windows PowerShell ISE über die Menüs (sofern verfügbar) sowie eine größere Auswahl von Befehlen aus dem Konsolenbereich über Skripterstellung ausführen können.

### So legen Sie einen Haltepunkt fest
Ein Haltepunkt kann in einem Skript nur festgelegt werden, nachdem es gespeichert wurde. Klicken Sie mit der rechten Maustaste auf die Zeile, für die Sie einen Zeilenhaltepunkt festlegen möchten, und klicken Sie dann auf **Haltepunkt umschalten**. Klicken Sie alternativ auf die Zeile, für die Sie einen Zeilenhaltepunkt festlegen möchten, und drücken Sie **F9**, oder klicken Sie im Menü **Debuggen** auf **Haltepunkt umschalten**.

Das folgende Skript ist ein Beispiel dazu, wie Sie aus dem Konsolenbereich einen Variablenhaltepunkt mit dem Cmdlet [Set-PSBreakpoint](https://technet.microsoft.com/library/6afd5d2c-a285-4796-8607-3cbf49471420) festlegen können.

``` PowerShell
# This command sets a breakpoint on the Server variable in the Sample.ps1 script.
Set-PSBreakpoint -Script sample.ps1 -Variable Server
```

### Auflisten aller Haltepunkte
Zeigt alle aktuell in der Windows PowerShell®-Sitzung vorhandenen Haltepunkte an.

Klicken Sie im Menü **Debuggen** auf **Haltepunkte auflisten**. Das folgende Skript ist ein Beispiel dazu, wie Sie aus dem Konsolenbereich alle Haltepunkte mit dem Cmdlet [Get-PSBreakpoint](https://technet.microsoft.com/library/0bf48936-00ab-411c-b5e0-9b10a812a3c6) auflisten können.

``` PowerShell
# This command lists all breakpoints in the current session. 
Get-PSBreakpoint
```

### Entfernen eines Haltepunkts
Durch Entfernen eines Haltepunkts wird dieser gelöscht.  Wenn Sie annehmen, dass Sie ihn später erneut verwenden möchten, bietet es sich an, ihn stattdessen zu [deaktivieren](#bkmk_disable).  Klicken Sie mit der rechten Maustaste auf die Zeile, aus der Sie einen Haltepunkt entfernen möchten, und klicken Sie dann auf **Haltepunkt umschalten**. Klicken Sie alternativ auf die Zeile, aus der Sie einen Haltepunkt entfernen möchten, und klicken Sie im Menü **Debuggen** auf **Haltepunkt umschalten**. Das folgende Skript ist ein Beispiel dazu, wie aus dem Konsolenbereich ein Haltepunkt mit angegebener ID mit dem Cmdlet [Remove-PSBreakpoint](https://technet.microsoft.com/library/4c877a80-0ea0-4790-9281-88c08ef0ddd6) entfernt werden kann.

``` PowerShell
# This command deletes the breakpoint with breakpoint ID 2.
Remove-PSBreakpoint -Id 2
```

### Alle Haltepunkte entfernen
Wenn Sie alle Haltepunkte entfernen möchten, die in der aktuellen Sitzung definiert sind, klicken Sie im Menü **Debuggen** auf **Alle Haltepunkte entfernen**.

Das folgende Skript ist ein Beispiel dazu, wie aus dem Konsolenbereich alle Haltepunkte mit dem Cmdlet [Remove-PSBreakpoint](https://technet.microsoft.com/library/4c877a80-0ea0-4790-9281-88c08ef0ddd6) entfernt werden können.

``` PowerShell
# This command deletes all of the breakpoints in the current session.
Get-PSBreakpoint | Remove-PSBreakpoint
```

### <a name="bkmk_disable"></a>Deaktivieren eines Haltepunkts
Deaktivieren eines Haltepunktes bewirkt wird nicht, dass er entfernt wird. Er wird dadurch deaktiviert, bis er wieder aktiviert wird.  Um einen bestimmten Zeilenhaltepunkt zu deaktivieren, klicken Sie auf die Zeile, in der Sie den Haltepunkt deaktivieren möchten, und klicken Sie dann auf **Haltepunkt deaktivieren**. Klicken Sie alternativ auf die Zeile, in der Sie einen Haltepunkt deaktivieren möchten, und drücken Sie **F9**, oder klicken Sie im Menü **Debuggen** auf **Haltepunkt deaktivieren**. Das folgende Skript ist ein Beispiel dazu, wie Sie aus dem Konsolenbereich einen Haltepunkt mit angegebener ID mit dem Cmdlet [Disable-PSBreakpoint](https://technet.microsoft.com/library/d4974e9b-0aaa-4e20-b87f-f599a413e4e8) entfernen können.

``` PowerShell
# This command disables the breakpoint with breakpoint ID 0.
Disable-PSBreakpoint -Id 0
```

### Deaktivieren aller Haltepunkte
Deaktivieren eines Haltepunktes bewirkt wird nicht, dass er entfernt wird. Er wird dadurch deaktiviert, bis er wieder aktiviert wird.  Wenn Sie alle Haltepunkte in der aktuellen Sitzung deaktivieren möchten, klicken Sie im Menü **Debuggen** auf **Alle Haltepunkte deaktivieren**. Das folgende Skript ist ein Beispiel dazu, wie Sie aus dem Konsolenbereich alle Haltepunkte mit dem Cmdlet [Disable-PSBreakpoint](https://technet.microsoft.com/library/d4974e9b-0aaa-4e20-b87f-f599a413e4e8) deaktivieren können.

``` PowerShell
# This command disables all breakpoints in the current session. 
# You can abbreviate this command as: "gbp | dbp".
Get-PSBreakpoint | Disable-PSBreakpoint
```

### Aktivieren eines Haltepunkts
Um einen bestimmten Zeilenhaltepunkt zu aktivieren, klicken Sie auf die Zeile, in der Sie den Haltepunkt aktivieren möchten, und klicken Sie dann auf **Haltepunkt aktivieren**. Klicken Sie alternativ auf die Zeile, in der Sie einen Haltepunkt aktivieren möchten, und drücken Sie **F9**, oder klicken Sie im Menü **Debuggen** auf **Haltepunkt aktivieren**. Das folgende Skript ist ein Beispiel dazu, wie Sie aus dem Konsolenbereich bestimmte Haltepunkte mit dem Cmdlet [Enable-PSBreakpoint](https://technet.microsoft.com/library/739e1091-3b3f-405f-a428-bec7543e5df0) aktivieren können.

``` PowerShell
# This command enables breakpoints with breakpoint IDs 0, 1, and 5.
Enable-PSBreakpoint -Id 0, 1, 5
```

### Aktivieren aller Haltepunkte
Wenn Sie alle Haltepunkte aktivieren möchten, die in der aktuellen Sitzung definiert sind, klicken Sie im Menü **Debuggen** auf **Alle Haltepunkte aktivieren**. Das folgende Skript ist ein Beispiel dazu, wie Sie aus dem Konsolenbereich alle Haltepunkte mit dem Cmdlet [Enable-PSBreakpoint](https://technet.microsoft.com/library/739e1091-3b3f-405f-a428-bec7543e5df0) aktivieren können.

``` PowerShell
# This command enables all breakpoints in the current session. 
# You can abbreviate the command by using their aliases: "gbp | ebp".
Get-PSBreakpoint | Enable-PSBreakpoint
```

## <a name="bkmk_2"></a>Verwalten einer Debugsitzung
Bevor Sie mit dem Debuggen beginnen, müssen Sie mindestens einen Haltepunkt festlegen. Einen Haltepunkt können Sie nur dann festlegen, wenn das Skript, das Sie debuggen möchten, gespeichert ist. Anleitungen zum Festlegen eines Haltepunkts finden Sie unter [Verwalten von Haltepunkten](#bkmk_1) oder [Set-PSBreakpoint](https://technet.microsoft.com/library/6afd5d2c-a285-4796-8607-3cbf49471420). Nachdem Sie mit dem Debuggen begonnen haben, können Sie ein Skript erst wieder bearbeiten, nachdem Sie das Debuggen beendet haben. Ein Skript, für das es mindestens einen Haltepunkt gibt, wird automatisch gespeichert, bevor es ausgeführt wird.

### So starten Sie das Debuggen
Drücken Sie **F5**, oder klicken Sie auf der Symbolleiste auf das Symbol **Skript ausführen**, oder klicken Sie im Menü **Debuggen** auf **Ausführen/Fortsetzen**. Das Skript wird ausgeführt, bis der erste Haltepunkt erreicht ist. Das Ausführen des Skripts wird in dieser Zeile angehalten, und die Zeile wird hervorgehoben angezeigt.

### So setzen Sie das Debuggen fort
Drücken Sie **F5**, oder klicken Sie auf der Symbolleiste auf das Symbol **Skript ausführen**, oder klicken Sie im Menü **Debuggen** auf **Ausführen/Fortsetzen**, oder geben Sie im Konsolenfenster **C** ein, und drücken Sie dann die **EINGABETASTE**. Dies bewirkt, dass das Skript weiter bis zum nächsten Haltepunkt oder bis zum Ende des Skripts ausgeführt wird, wenn keine weiteren Haltepunkte gefunden werden.

### So zeigen Sie die Aufrufliste an
Die Aufrufliste zeigt die aktuelle Ausführungsstelle im Skript an. Wird im Skript eine Funktion ausgeführt, die aus einer anderen Funktion aufgerufen wurde, wird dies in der Anzeige durch zusätzliche Zeilen in der Ausgabe dargestellt. In der untersten Zeile werden das ursprüngliche Skript und dessen Zeile angezeigt, in der die Funktion aufgerufen wurde. In der Zeile darüber werden die Funktion und die Zeile angezeigt, in der möglicherweise eine weitere Funktion aufgerufen wurde.  In der obersten Zeile wird der aktuelle Kontext der aktuellen Zeile angezeigt, für die der Haltepunkt festgelegt ist.

Um bei angehaltener Skriptausführung die aktuelle Aufrufliste anzuzeigen, drücken Sie **STRG+UMSCHALT+D**, oder klicken Sie im Menü **Debuggen** auf **Aufrufliste anzeigen**, oder geben Sie im Konsolenbereich **K** ein, und drücken Sie dann die **EINGABETASTE**.

### So beenden Sie das Debuggen
Drücken Sie **UMSCHALT+F5**, oder klicken Sie im Menü **Debuggen** auf **Debugger beenden**, oder geben Sie im Konsolenbereich **K** ein, und drücken Sie dann die **EINGABETASTE**.

## <a name="bkmk_3"></a>Schrittweises Debuggen: Überspringen, Einzelschritt und Rücksprung
Schrittweises Debuggen ist die Vorgehensweise, bei der immer nur jeweils eine Zeile ausgeführt wird. Sie können in einer Codezeile anhalten und die Werte von Variablen sowie den Status des Systems prüfen. In der folgenden Tabelle sind allgemeinen Debugaufgaben wie Überspringen, Einzelschritt und Rücksprung beschrieben.

| Debugaufgabe | Beschreibung | Vorgehensweise in PowerShell ISE |
| --- | --- | --- |
| **Einzelschritt** | Führt die aktuelle Anweisung aus und hält dann bei der nächsten Anweisung an. Ist die aktuelle Anweisung ein Funktions- oder Skriptaufruf, wechselt der Debugger in diese Funktion oder dieses Skript. Andernfalls hält er bei der nächsten Anweisung an. | Drücken Sie **F11**, oder klicken Sie im Menü **Debuggen** auf **Einzelschritt**, oder geben Sie im Konsolenbereich **S** ein, und drücken Sie die **EINGABETASTE**. |
| **Nächster Schritt** | Führt die aktuelle Anweisung aus und hält dann bei der nächsten Anweisung an. Ist die aktuelle Anweisung ein Funktions- oder Skriptaufruf, führt der Debugger die gesamte Funktion oder das gesamte Skript aus und hält bei der Anweisung an, die auf den Funktions- oder Skriptaufruf folgt. | Drücken Sie **F10**, oder klicken Sie im Menü **Debuggen** auf **Überspringen**, oder geben Sie im Konsolenbereich **V** ein, und drücken Sie die **EINGABETASTE**. |
| **Ausführen bis Rücksprung** | Führt einen Rücksprung aus der aktuellen Funktion und auf eine Ebene höher aus, wenn die Funktion geschachtelt ist. Befindet sich der Fokus im Hauptteil, wird das Skript bis zum Ende oder bis zum nächsten Haltepunkt ausgeführt. Die übersprungenen Anweisungen werden ausgeführt, aber nicht in Einzelschritten durchlaufen. | Drücken Sie **UMSCHALT+F11**, oder klicken Sie im Menü **Debuggen** auf **Rücksprung**, oder geben Sie im Konsolenbereich **O** ein, und drücken Sie die **EINGABETASTE**. |
| **Fortsetzen** | Setzt die Ausführung bis zum Ende oder bis zum nächsten Haltepunkt fort. Die übersprungenen Funktionen und Aufrufe werden ausgeführt, aber nicht in Einzelschritten durchlaufen. | Drücken Sie **F5**, oder klicken Sie im Menü **Debuggen** auf **Ausführen/Fortsetzen**, oder geben Sie im Konsolenbereich **C** ein, und drücken Sie die **EINGABETASTE**. |

## <a name="bkmk_4"></a>Anzeigen der Werte von Variablen beim Debuggen
Sie können die aktuellen Werte von Variablen im Skript anzeigen, während Sie den Code schrittweise durchlaufen.

### So zeigen Sie die Werte von Standardvariablen an
Verwenden Sie eine der folgenden Methoden:

-   Zeigen Sie im Skriptbereich auf die jeweilige Variable, um deren Wert als QuickInfo anzuzeigen.

-   Geben Sie im Konsolenbereich den Variablennamen ein, und drücken Sie die **EINGABETASTE**.

Alle Bereiche in ISE befinden sich immer im selben Geltungsbereich. Daher werden die Befehle, die Sie beim Debuggen eines Skripts im Konsolenbereich eingeben, im Geltungsbereich des Skripts ausgeführt. Dies ermöglicht es Ihnen, im Konsolenbereich nach den Werten von Variablen zu suchen und Funktionen aufzurufen, die nur im Skript definiert sind.

### So zeigen Sie die Werte von automatischen Variablen an
Mit den vorherigen Methoden können Sie die Werte fast aller Variablen anzeigen, während Sie ein Skript debuggen. Für die folgenden automatischen Variablen funktionieren diese Methoden jedoch nicht.

-   $_

-   $Input

-   $MyInvocation

-   $PSBoundParameters

-   $Args

Wenn Sie den Wert von einer dieser Variablen anzeigen, erhalten Sie den Wert, den diese Variablen für eine interne, vom Debugger verwendete Pipeline hat, nicht den Wert der Variablen im Skript. Sie können dieses Problem für einige Variablen umgehen ($_, $Input, $MyInvocation, $PSBoundParameters und $Args), indem Sie wie folgt vorgehen:

1.  Weisen Sie im Skript den Wert der automatischen Variablen einer neuen Variablen zu.

2.  Zeigen Sie den Wert der neuen Variablen an, indem Sie im Skriptbereich mit der Maus auf sie zeigen oder sie im Konsolenbereich eingeben.

Möchten Sie beispielsweise den Wert der Variablen „$MyInvocation“ anzeigen, weisen Sie deren Wert im Skript einer neuen Variablen zu, etwa „$scriptname“, und zeigen Sie dann mit dem Mauszeiger auf die Variable „$scriptname“, oder geben Sie diese ein.

``` PowerShell
#In MyScript.ps1
$scriptname = $MyInvocation.MyCommand.Path

#In the Console Pane:
C:\ps-test> $scriptname
C:\ps-test\MyScript.ps1
```

## Weitere Informationen
[Mithilfe von Windows PowerShell ISE](Using-the-Windows-PowerShell-ISE.md)




<!--HONumber=Oct16_HO1-->


