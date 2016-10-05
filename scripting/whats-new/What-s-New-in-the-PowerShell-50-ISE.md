---
title: Neuerungen bei der Windows PowerShell 5.0 ISE
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 38648d47-7c27-4b37-a40e-ad29948519c2
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: ababa1b3ce913528a3ac7089d91ef74c5eb27737

---

# Neuerungen bei der Windows PowerShell ISE
In diesem Thema werden die neuen und aktualisierten Features vorgestellt, die in Versionen von Windows PowerShell® Integrated Scripting Environment (ISE) eingeführt wurden.

## <a name="overview"></a>Featurebeschreibung
Die Windows PowerShell ISE ist eine Hostanwendung, die es Ihnen ermöglicht, Skripts und Module in einer grafischen und intuitiven Umgebung zu schreiben, auszuführen und zu testen. Wichtige Features wie farbliche Syntaxkennzeichnung, Vervollständigung mit der TAB-Taste, visuelles Debuggen, Unicode-Kompatibilität und kontextbezogene Hilfe ermöglichen eine komfortable Skripterstellung.

Eine Übersicht über die Windows PowerShell ISE finden Sie unter [Windows PowerShell Integrated Scripting Environment (Übersicht)](https://technet.microsoft.com/en-us/library/3c1892c2-bf84-4cb6-af26-1f453be9e671).

## <a name="versions"></a>Neue und geänderte Funktionen in der Windows PowerShell ISE
Die folgende Tabelle enthält die neuen und geänderten Funktionen für diese Version von Windows PowerShell ISE in Windows PowerShell.

|Feature/Funktionalität|Windows PowerShell ISE 4.0|Windows PowerShell ISE 3.0|Windows PowerShell ISE 2.0|
|--------------------------|-----------------------------------------------|-----------------------------------------------|-----------------------------------------------|
|**[IntelliSense](#BKMK_Intellisense)**|X|X||
|**[Codeausschnitte](#bkmk_snippets)**|X|X||
|**[Add-On-Tools](#BKMK_AddOnTools)**|X|X||
|**[Neustart-Manager und automatisches Speichern](#BKMK_RestartMgr)**|X|X||
|**[Konsolenbereich](#BKMK_ConsolePane)**|X|X||
|**[Liste der zuletzt verwendeten](#BKMK_MRU)**|X|X||
|**[Befehlszeilenoptionen](#BKMK_CommandLine)**|X|X||
|**[Neue Features im editor](#BKMK_NewEditorFeatures)**|X|X||
|**[Neue Hilfe-Viewer-Fenster](#BKMK_NewHelpViewer)**|X|X||
|**[Show-Command-cmdlet](#BKMK_ShowCommand)**|X|X||

### <a name="BKMK_Intellisense"></a>IntelliSense
**In der ISE 3.0 hinzugefügt**

IntelliSense ist ein Feature für die automatische Vervollständigung, das zur Windows PowerShell ISE gehört. IntelliSense zeigt während der Eingabe klickbare Menüs möglicherweise übereinstimmender Cmdlets, Parameter, Parameterwerte, Dateien oder Ordner an.

**Welchen Nutzen hinzufügen diese Änderung?**

Durch Hinzufügen von IntelliSense ist es einfacher, Cmdlets und Syntax zu ermitteln, wenn Sie die Windows PowerShell ISE zum Erstellen von Skripts nutzen. Sie können mithilfe von Windows PowerShell ISE auch Windows PowerShell erlernen, während Sie neue Skripts erstellen.

**Worin bestehen die Unterschiede?**

Bei Eingabe von Cmdlets in die Windows PowerShell ISE 3.0 oder höher wird ein bildlauffähiges und klickbares Menü angezeigt, das Ihnen das Durchsuchen und Auswählen der entsprechenden Befehle ermöglicht.

### <a name="BKMK_Snippets"></a>Codeausschnitte
**In der ISE 3.0 hinzugefügt**

*Codeausschnitte* sind kurze Abschnitte von Windows PowerShell-Code, die Sie in die in Windows PowerShell ISE erstellten Skripts einfügen können. Windows PowerShell ISE bietet einen Standardsatz von Codeausschnitten. Beim Arbeiten in der Windows PowerShell ISE können Sie Codeausschnitte mithilfe des Cmdlets **New-Snippet** hinzufügen.

**Welchen Nutzen hinzufügen diese Änderung?**

Mithilfe von Codeausschnitten können Sie Skripts zum Automatisieren Ihrer Umgebung schnell zusammenstellen und erstellen.

**Worin bestehen die Unterschiede?**

Klicken Sie zum Verwenden von Codeausschnitten in Windows PowerShell 3.0 oder höher im Menü **Bearbeiten** auf **Ausschnitte starten**, oder drücken Sie **STRG+J**.

### <a name="BKMK_AddOnTools"></a>Add-On-tools
**Hinzugefügt in PowerShell 3.0**

Die Windows PowerShell ISE unterstützt jetzt Add-On-Tools, bei denen es sich um WPF-Steuerelemente (Windows Presentation Foundation) handelt, die mithilfe des Objektmodells hinzugefügt werden. Add-On-Tools können in der Konsole in einem vertikalen oder horizontalen Bereich angezeigt werden. Mehrere Add-On-Tools in einem Bereich werden als Registerkarten-Steuerelement angezeigt. Sie können auch Add-On-Tools von anderen Anbietern als Microsoft hinzufügen oder entfernen. Weitere Informationen zum Importieren oder Entfernen von Add-On-Tools finden Sie unter [Windows PowerShell ISE Operations](http://technet.microsoft.com/library/cc732148.aspx) (Windows PowerShell ISE-Vorgänge).

**Welchen Nutzen hinzufügen diese Änderung?**

Add-Ons ermöglichen Ihnen das Erweitern und Anpassen der Windows PowerShell ISE mit Tools, die Ihre Skripterstellungsumgebung verbessern oder der Windows PowerShell ISE neue Funktionen hinzufügen können.

**Worin bestehen die Unterschiede?**

Zum Funktionsumfang der Windows PowerShell ISE 3.0 und höher gehört das Add-On **Befehle**. Das Add-On **Befehle** ermöglicht das Durchsuchen von Cmdlets und das Zugreifen auf die Hilfe zu den Cmdlets. Es wird neben den Bereichen **Skript** und **Konsole** angezeigt.

Zusätzliche Add-Ons finden Sie über den Befehl **Website mit Add-On-Tools öffnen** im Menü**Add-Ons**.

### <a name="BKMK_RestartMgr"></a>Neustart-Manager und automatisches Speichern
**Hinzugefügt in PowerShell 3.0**

Windows PowerShell ISE speichert nun Ihre geöffneten Skripts automatisch alle zwei Minuten an einem gesonderten Speicherort.  Wenn Windows PowerShell ISE unerwartet beendet oder das Betriebssystem neu gestartet wird, stellt Windows PowerShell ISE die in der letzten Sitzung geöffneten Skripts wieder her (auch wenn sie nicht gespeichert wurden).

Führen Sie im Konsolenbereich den folgenden Befehl aus, um das Intervall für die automatische Speicherung zu ändern: **$psise.Options.AutoSaveMinuteInterval**.

**Welchen Nutzen hinzufügen diese Änderung?**

Sie können jetzt mit Windows PowerShell ISE mit dem Wissen arbeiten, dass Ihre geöffneten Skripts im Fall eines unerwarteten Neustarts automatisch gespeichert werden.

**Worin bestehen die Unterschiede?**

In Windows PowerShell ISE 2.0 werden die Skripts bei einem Neustart nicht automatisch gespeichert.

### <a name="BKMK_MRU"></a>Liste der zuletzt verwendeten
**Hinzugefügt in PowerShell 3.0**

Die Windows PowerShell ISE bietet jetzt eine Liste der zuletzt verwendeten Dateien. Beim Öffnen einer Datei in der Windows PowerShell ISE wird die Datei der Liste „Zuletzt verwendet“ im Menü **Datei** hinzugefügt.

Führen Sie im Konsolenbereich den folgenden Befehl aus, um die Standardanzahl von Dateien in der Liste „Zuletzt verwendet“ zu ändern: **$psise.Options.MruCount**.

**Welchen Nutzen hinzufügen diese Änderung?**

Über die Liste „Zuletzt verwendet“ können Sie nun mühelos auf die zuletzt verwendeten Dateien zugreifen.

**Worin bestehen die Unterschiede?**

Die Windows PowerShell ISE 2.0 bietet keine solche Liste.

### <a name="BKMK_ConsolePane"></a>Konsolenbereich
**Hinzugefügt in PowerShell 3.0**

Die separaten Befehls- und Ausgabebereiche, die in der ersten Version der Windows PowerShell ISE verfügbar waren, wurden in einem Konsolenbereich kombiniert. Hinsichtlich Funktion und Darstellung ähnelt der Konsolenbereich einer typischen Windows PowerShell-Konsole, bietet aber die im Folgenden aufgeführten Erweiterungen (von denen die meisten in diesem Thema beschrieben werden).

-   Syntaxfarben für Eingabetext (nicht für Ausgabetext), einschließlich XML-Syntax

-   IntelliSense

-   Zugehörige Klammer

-   Fehleranzeige

-   Vollständige Unicode-Unterstützung

-   Kontextbezogene Hilfe über **F1**

-   Kontextbezogene Befehlsanzeige (Show-Command) über **STRG+F1**

-   Unterstützung für komplexe Skripts und Sprachen mit Leserichtung von rechts nach links

-   Schriftartenunterstützung

-   Zoom

-   Modi für Linien- und Blockauswahl

-   Beibehaltung von über die Befehlszeile eingegebenem Inhalt, wenn Sie mithilfe der NACH-OBEN-TASTE** **den Verlauf in der Konsole anzeigen

**Welchen Nutzen hinzufügen diese Änderung?**

Diese Änderungen im Konsolenbereich ermöglichen eine Skripterstellungsumgebung, die mit der Konsolenschnittstelle konsistenter ist.

**Worin bestehen die Unterschiede?**

Die Windows PowerShell ISE 2.0 verfügt über getrennte Befehls- und Ausgabebereiche.

### <a name="BKMK_CommandLine"></a>Befehlszeilenoptionen
**Hinzugefügt in PowerShell 3.0**

Wenn Sie die Windows PowerShell ISE über die Befehlszeile starten (indem Sie **powershell_ise.exe** eingeben), können Sie die folgenden neuen Befehlszeilenschalter hinzufügen.

-   *-NoProfile*: Startet die Windows PowerShell ISE ohne Ausführung von **$profile**

-   *-Help*: Zeigt ein Hilfefenster an

-   *-mta*: Startet die Windows PowerShell ISE im Multithread-Apartment-Modus Der Standardbetriebsmodus der Windows PowerShell ISE ist der Singlethread-Apartment-Modus bzw. *-sta*.

**Welchen Nutzen hinzufügen diese Änderung?**

Das Hinzufügen dieser Befehlszeilenschalter ermöglicht Ihnen das Steuern der Umgebung, in der die Windows PowerShell ISE ausgeführt wird.

**Worin bestehen die Unterschiede?**

Die Windows PowerShell ISE 2.0 erkennt diese Befehlszeilenschalter nicht.

### <a name="BKMK_NewEditorFeatures"></a>Neue Features im Editor
**Hinzugefügt in PowerShell 3.0**

Zu den weiteren Windows PowerShell ISE-Bearbeitungsfeatures zählen:

-   **XML-Syntaxfarben** Windows PowerShell ISE versieht die XML-Syntax jetzt ebenso mit Farben wie die Windows PowerShell-Syntax.

-   **Zugehörige Klammer** Die Windows PowerShell ISE bietet die Funktion „Zugehörige Klammer (Hervorhebung)“, die wie folgt verwendet werden kann: Wenn Sie z.B. den Befehl **Gehe zu Spiel** bzw. **STRG+]** verwenden, wird die schließende Klammer gefunden, wenn eine öffnende Klammer ausgewählt ist.

-   **Gliederungsansicht** Der Skriptbereich unterstützt Gliederungen, sodass Codeabschnitte durch Klicken auf Plus- bzw. Minuszeichen am linken Rand auf- bzw. zugeklappt werden können. Sie können Klammern bzw. die Tags **#region** und **#endregion** verwenden, um den Anfang bzw. das Ende eines zuklappbaren Abschnitts zu markieren. Drücken Sie zum Auf- bzw. Zuklappen aller Bereiche **STRG+M**.

-   **Textbearbeitung mit Drag & Drop** Windows PowerShell ISE unterstützt nun die Textbearbeitung mit Drag & Drop. Sie können einen beliebigen Textblock auswählen und an eine andere Stelle im Editor oder in der Konsole ziehen, um den Text zu verschieben. Wenn Sie die STRG-Taste gedrückt halten, während Sie den markierten Text ziehen, wird der Text, sobald Sie die Maustaste loslassen, an den neuen Ort kopiert. In dieser Version von Windows PowerShell ISE wird wie in der vorherigen Version von Windows PowerShell ISE beim Ziehen und Ablegen von Dateien auf Windows PowerShell ISE die Datei von Windows PowerShell ISE geöffnet.

-   **Anzeige von Analysefehlern** Analysefehler werden rot unterstrichen angezeigt. Wenn Sie auf einen Fehler zeigen, wird das im Code gefundene Problem als QuickInfo angezeigt.

-   **Zoom** Der Zoomprozentsatz des Konsoleninhalts kann mit einem Zoomschieberegler (rechts unten im Fenster der Windows PowerShell ISE) oder durch Eingabe des Befehls **$psise.options.Zoom** im Konsolenbereich festgelegt werden.

-   **Umfassende Funktionen zum Kopieren und Einfügen von Text** Beim Kopieren in die Zwischenablage in Windows PowerShell ISE bleiben Informationen zur Schriftart, Größe und Farbe der ursprünglichen Auswahl erhalten.

-   **Blockauswahl** Sie können einen Textblock auswählen, indem Sie bei gedrückter ALT-TASTE den Text im Skriptbereich mit der Maus auswählen oder **ALT+UMSCHALT+NACH-OBEN/NACH-UNTEN** drücken.

**Welchen Nutzen hinzufügen diese Änderung?**

Die zusätzlichen Bearbeitungsfeatures bieten eine konsistentere und leistungsstärkere Bearbeitungsumgebung.

**Worin bestehen die Unterschiede?**

In Windows PowerShell ISE 2.0 waren diese Bearbeitungsoptimierungen nicht vorhanden.

### <a name="BKMK_NewHelpViewer"></a>Neues Anzeigefenster für Hilfe
**Hinzugefügt in PowerShell 3.0**

Wenn Sie **F1** drücken, während sich der Cursor in einem Cmdlet befindet oder Sie einen Teil eines Cmdlets hervorgehoben haben, wird im neuen Anzeigefenster für Hilfe eine kontextbezogene Hilfe zum hervorgehobenen Cmdlet geöffnet. Um die konzeptionelle Hilfe für Windows PowerShell ISE anzuzeigen, geben Sie im Konsolenbereich **operators** ein, und drücken Sie dann **F1**.

Laden Sie die aktuelle Version der Windows PowerShell-Hilfethemen von der Microsoft-Website herunter, bevor Sie dieses Feature verwenden. Die einfachste Methode zum Herunterladen der Hilfethemen ist die Ausführung des Cmdlets **Update-Help** im Konsolenbereich, wenn Sie die Windows PowerShell ISE als Administrator ausführen.

Sie können ändern, wo über die Taste **F1** nach Hilfe gesucht wird. Im Menü **Extras**/**Optionen** können Sie auf der Registerkarte **Allgemeine Einstellungen** unter **Andere Einstellungen** das Kontrollkästchen **Lokale Hilfe anstatt Onlineinhalt verwenden** aktivieren bzw. deaktivieren. Falls aktiviert, sucht der Client die Cmdlet-Hilfe in der heruntergeladenen Hilfe im Ordner „modules“.  Falls das Kontrollkästchen deaktiviert ist, sucht der Client in der TechNet-Bibliothek nach der Cmdlet-Hilfe.

**Welchen Nutzen hinzufügen diese Änderung?**

Durch eine kontextbezogene Hilfe ohne Verlassen des aktuellen Cmdlets oder Skripts erhalten Sie eine flüssige Lernerfahrung.

**Worin bestehen die Unterschiede?**

Durch Drücken von F1 in früheren Versionen von Windows PowerShell ISE wurde die Hilfedatei auf dem lokalen Computer geöffnet. In Windows PowerShell ISE 3.0 und höher wird ein Fenster geöffnet, das die Hilfe für das Cmdlet enthält, die durchsuchbar und konfigurierbar ist. Diese Hilfe ist in Windows PowerShell ISE 3.0 neu. Die aktualisierbare Hilfe ist in Windows PowerShell 3.0 neu.

### <a name="BKMK_ShowCommand"></a>Show-Command-cmdlet
**Hinzugefügt in PowerShell 3.0**

Das Cmdlet **Show-Command** ermöglicht das Erstellen und Ausführen eines Cmdlets oder einer Funktion, indem ein grafisches Formular ausgefüllt wird. Das Formular ermöglicht Benutzern das Arbeiten mit Windows PowerShell in einer grafischen Umgebung. Mit **Show-Command** können erfahrene Skriptentwickler schnell eine Windows PowerShell-basierte grafische Benutzeroberfläche (GUI) erstellen.

**Welchen Nutzen hinzufügen diese Änderung?**

Durch Verwenden von **Show-Command** in Ihren Windows PowerShell-Skripts können Sie Ihren Benutzern die grafische Umgebung bereitstellen, mit der sie vertraut sind. Darüber hinaus hilft **Show-Command** Einsteigern beim Erlernen von Windows PowerShell.

**Worin bestehen die Unterschiede?**

„Show-Command“ ist in der Windows PowerShell ISE 3.0 neu.

## <a name="BKMK_LINKS"></a>Siehe auch
Weitere Informationen zur Verwendung von Windows PowerShell ISE in Windows PowerShell finden Sie unter den folgenden Links.

-   [Mithilfe der Windows PowerShell Integrated Scripting Environment](../core-powershell/ise/Using-the-Windows-PowerShell-ISE.md)

-   [ISE im TechNet-Wiki](http://social.technet.microsoft.com/wiki/search/searchresults.aspx?q=ISE)

-   [Script Center](http://technet.microsoft.com/scriptcenter/default)




<!--HONumber=Oct16_HO1-->


