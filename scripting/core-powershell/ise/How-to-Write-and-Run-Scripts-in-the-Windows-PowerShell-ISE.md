---
title: "Schreiben und Ausführen von Skripts in der Windows PowerShell ISE"
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 62f916d9-b3a1-484a-bdfb-41f57112c22b
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 6e58c298725254a84819856070b9702340fcf81d

---

# Schreiben und Ausführen von Skripts in der Windows PowerShell ISE
In diesem Thema ist beschrieben, wie Skripts im Skriptbereich erstellt, bearbeitet, ausgeführt und gespeichert werden.

-   [Gewusst wie: Erstellen und Ausführen von Skripts](#bkmk_1)

-   [Das Schreiben und Bearbeiten von Text in den Skriptbereich](#bkmk_2)

-   [Gewusst wie: Speichern Sie ein Skript](#bkmk_3)

## <a name="bkmk_1"></a>Erstellen und Ausführen von Skripts
Sie können Windows PowerShell®-Dateien im Skriptbereich öffnen und bearbeiten. Die speziellen Dateitypen für Windows PowerShell® sind Skriptdateien (.ps1), Skriptdatendateien (.psd1) und Skriptmoduldateien (.psm1). Diese Dateitypen werden mit Syntaxfärbung im Skriptbereichs-Editor angezeigt. Andere gängige Dateitypen, die Sie möglicherweise im Skriptbereich öffnen möchten, sind Konfigurationsdateien (PS1XML), XML-Dateien und Textdateien.

> [!NOTE]
> Die Windows PowerShell-Ausführungsrichtlinie bestimmt, ob Sie Skripts ausführen sowie Windows PowerShell-Profile und -Konfigurationsdateien laden können. Die Standardausführungsrichtlinie, „Restricted“, verhindert sowohl das Ausführen jeglicher Skripts als auch das Laden von Profilen. Wenn Sie die Ausführungsrichtlinie ändern möchten, sodass sie das Laden und Verwenden von Profilen zulässt, lesen Sie [Set-ExecutionPolicy[PSITPro5_Security]](https://technet.microsoft.com/en-us/library/5690a0e1-495b-4e63-8280-65ead7bf01ab) und [about_Signing [v4]](https://technet.microsoft.com/en-us/library/fcbdd3b9-0b9f-4734-b5c7-e0dcc304fa1d).

### So erstellen Sie eine neue Skriptdatei
Klicken Sie auf der Symbolleiste auf **Neu**, oder klicken Sie im Menü **Datei** auf **Neu**. Die erstellte Datei wird in einer neuen Dateiregisterkarte unter der aktuellen PowerShell-Registerkarte angezeigt. Denken Sie daran, dass die PowerShell-Registerkarten nur angezeigt werden, wenn mehrere vorhanden sind. Standardmäßig wird eine Datei des Typs Skript (PS1) erstellt, diese kann aber mit einem neuen Namen und einer neuen Erweiterung gespeichert werden. Es können mehrere Skriptdateien auf derselben PowerShell-Registerkarte erstellt werden.

### So öffnen Sie ein vorhandenes Skript
Klicken Sie auf der Symbolleiste auf **Öffnen…**, oder klicken Sie im Menü **Datei** auf **Öffnen**. Wählen Sie im Dialogfeld **Öffnen** die Datei aus, die Sie öffnen möchten. Die geöffnete Datei wird auf einer neuen Registerkarte angezeigt.

### So schließen Sie eine Skriptregisterkarte
Klicken Sie auf die Skriptregisterkarte des Skripts, das Sie schließen möchten, und führen Sie dann einen der folgenden Schritte aus:

1.  Klicken Sie auf das Symbol **Schließen** (X) auf der Skriptregisterkarte.

2.  Klicken Sie im Menü **Datei** auf **Schließen**.

Wurde die Datei seit ihrer letzten Speicherung geändert, werden Sie aufgefordert, die Datei zu speichern oder zu verwerfen.

### So zeigen Sie den Dateipfad an
Zeigen Sie auf der Registerkarte der Datei auf den Dateinamen an. Der vollqualifizierte Pfad der Skriptdatei wird als QuickInfo angezeigt.

### So führen Sie ein Skript aus
Klicken Sie auf der Symbolleiste auf **Skript ausführen**, oder klicken Sie im Menü **Datei** auf **Ausführen**.

### So führen Sie einen Abschnitt eines Skripts aus

1.  Wählen Sie Skriptbereich einen Abschnitt eines Skripts aus.

2.  Klicken Sie im Menü **Datei** auf **Auswahl ausführen**, oder klicken Sie auf der Symbolleiste auf **Auswahl ausführen**.

### So beenden Sie ein Skript, das ausgeführt wird
Klicken Sie auf der Symbolleiste auf **Vorgang beenden**, drücken Sie STRG+UNTBR, oder klicken Sie im Menü **Datei** auf **Vorgang beenden**. Drücken von **STRG+C** funktioniert ebenfalls, sofern aktuell kein Text ausgewählt ist. Ist dies der Fall, wird **STRG+C** der Kopierfunktion für den ausgewählten Text zugeordnet.

## <a name="bkmk_2"></a>Schreiben und Bearbeiten von Text im Skriptbereich
Führen Sie die folgenden Schritte aus, um Text im Skriptbereich zu bearbeiten. Sie können Text kopieren, ausschneiden, einfügen, suchen und ersetzen. Sie können außerdem die letzte von Ihnen ausgeführte Aktion rückgängig machen und wiederholen. Die Tastenkombinationen zum Ausführen dieser Aktionen sind mit denen identisch, die für alle Windows-Anwendungen verwendet werden.

### So geben Sie Text im Skriptbereich ein

1.  Bewegen Sie den Cursor in den Skriptbereich, indem Sie auf eine beliebige Stelle im Skriptbereich oder im Menü **Ansicht** auf **Zum Skriptbereich gehen** klicken.

2.  Erstellen Sie ein Skript. Farbliche Syntaxkennzeichnung und Vervollständigung mit der TAB-TASTE vereinfachen dies erheblich in Windows PowerShell ISE.

3.  Ausführliche Informationen dazu, wie das Nutzen der Funktion Vervollständigung mit der TAB-TASTE beim Eingeben helfen kann, finden Sie unter [Verwenden von Vervollständigung mit der TAB-TASTE im Skriptbereich und Konsolenbereich](How-to-Use-Tab-Completion-in-the-Script-Pane-and-Console-Pane.md).

### So suchen Sie nach Text im Skriptbereich

1.  Drücken Sie **STRG+F**, oder klicken Sie im Menü **Bearbeiten** auf **Im Skript suchen**, um nach Text an einer beliebigen Stelle zu suchen.

2.  Um nach Text ab dem Cursor zu suchen, drücken Sie **F3**, oder klicken Sie im Menü **Bearbeiten** auf **Im Skript weitersuchen**.

3.  Drücken Sie **UMSCHALT+F3**, oder klicken Sie im Menü **Bearbeiten** auf **Vorheriges im Skript suchen**, um nach Text vor dem Cursor zu suchen.

### So finden und ersetzen Sie Text im Skriptbereich
Drücken Sie **STRG+H**, oder klicken Sie im Menü **Bearbeiten** auf **Im Skript ersetzen**. Geben Sie sowohl den zu suchenden Text und auch den Text ein, durch den der zu suchende Text ersetzt werden soll, und drücken Sie die **EINGABETASTE**.

### So wechseln Sie zu einer bestimmten Zeile des Texts im Skriptbereich

1.  Drücken Sie im Skriptbereich die Tastenkombination **STRG+G**, oder klicken Sie im Menü **Bearbeiten** auf **Gehe zu Zeile**.

2.  Geben Sie eine Zeilennummer ein.

### So kopieren Sie Text in den Skriptbereich

1.  Wählen Sie im Skriptbereich den Text aus, den Sie kopieren möchten.

2.  Drücken Sie **STRG+C**, klicken Sie auf der Symbolleiste auf das Symbol **Kopieren**, oder klicken Sie im Menü **Bearbeiten** auf **Kopieren**.

### So schneiden Sie Text im Skriptbereich aus

1.  Wählen Sie im Skriptbereich den Text aus, den Sie ausschneiden möchten.

2.  Drücken Sie **STRG+X**, oder klicken Sie auf der Symbolleiste auf das Symbol **Ausschneiden**, oder klicken Sie im Menü **Bearbeiten** auf **Ausschneiden**.

### So fügen Sie Text in den Skriptbereich ein
Drücken Sie **STRG+V**, oder klicken Sie auf der Symbolleiste auf das Symbol **Einfügen**, oder klicken Sie im Menü **Bearbeiten** auf **Einfügen**.

### So machen Sie eine Aktion im Skriptbereich rückgängig
Drücken Sie **STRG+Z**, oder klicken Sie auf der Symbolleiste auf das Symbol **Rückgängig**, oder klicken Sie im Menü **Bearbeiten** auf **Rückgängig**.

### So wiederholen Sie eine Aktion im Skriptbereich
Drücken Sie **STRG+Y**, oder klicken Sie auf der Symbolleiste auf das Symbol **Wiederholen**, oder klicken Sie im Menü **Bearbeiten** auf **Wiederholen**.

## <a name="bkmk_3"></a>Speichern eines Skripts
Gehen Sie folgendermaßen vor, um einem Skript einen Namen zu geben und es zu speichern. Ein Sternchen wird neben dem Skriptnamen angezeigt, um zu kennzeichnen, dass die Datei nicht gespeichert wurde, seit sie geändert wurde. Das Sternchen verschwindet, wenn die Datei gespeichert wird.

### So speichern Sie ein Skript
Drücken Sie **STRG+S**, oder klicken Sie auf der Symbolleiste auf das Symbol **Speichern**, oder klicken Sie im Menü **Datei** auf **Speichern**.

### So geben Sie einem Skript einen Namen und speichern es

1.  Klicken Sie im Menü **Datei** auf **Speichern unter**. Das Dialogfeld **Speichern unter** wird angezeigt.

2.  Geben Sie in das Feld **Dateiname** einen Namen für die Datei ein.

3.  Wählen Sie im Feld **Dateityp** einen Dateityp aus. Wählen Sie beispielsweise im Feld **Dateityp** den Typ „PowerShell-Skripte (*.ps1)“ aus.

4.  Klicken Sie auf **Speichern**.

### So speichern Sie ein Skript in ASCII-Codierung
Windows PowerShell ISE speichert neue Skriptdateien (.ps1), Skriptdatendateien (.psd1) und Skriptmoduldateien (.psm1) standardmäßig im Unicode-Format (BigEndianUnicode). Um ein Skript in einer anderen Codierung, z. B. ASCII (ANSI), zu speichern, verwenden Sie die **Save**- oder **SaveAs**-Methode des [$psISE.CurrentFile](https://technet.microsoft.com/en-us/library/bc3300e4-9c17-4f00-a621-c8867126e3b3#CurrentFile)-Objekts.

Im folgenden Befehl wird ein neues Skript als „MyScript.ps1“ mit ASCII-Codierung gespeichert.

```
$psise.CurrentFile.SaveAs("MyScript.ps1", [System.Text.Encoding]::ASCII)
```

Im folgenden Befehl wird die aktuelle Skriptdatei durch eine Datei ersetzt, die denselben Namen hat, aber in ASCII-Codierung vorliegt.

```
$psise.CurrentFile.Save([System.Text.Encoding]::ASCII)
```

Im folgenden Befehl wird die Codierung der aktuellen Datei abgerufen.

```
$psise.CurrentFile.encoding
```

Windows PowerShell ISE unterstützt die folgenden Codierungsoptionen: ASCII, BigEndianUnicode, Unicode, UTF32, UTF7, UTF8 und Default. Der Wert der Option „Default“ ist je nach System unterschiedlich.

Windows PowerShell ISE ändert nicht die Codierung von Skripts, die in anderen Editoren erstellt wurden. Dies gilt selbst dann, wenn Sie den Befehl „Speichern“ oder „Speichern unter“ in Windows PowerShell ISE verwenden.

## Weitere Informationen
[Mithilfe von Windows PowerShell ISE](Using-the-Windows-PowerShell-ISE.md)




<!--HONumber=Oct16_HO1-->


