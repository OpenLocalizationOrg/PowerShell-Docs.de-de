---
title: Kennenlernen der Windows PowerShell ISE
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: e0d2c6e8-5126-40e7-a1e1-d1cff29fe94a
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: c1f285a1d1beb35c931c17f5944d7ec697cb152e

---

# Kennenlernen der Windows PowerShell ISE
Sie können Windows PowerShell® Integrated Scripting Environment (ISE) verwenden, um Befehle und Skripts zu erstellen, auszuführen und zu debuggen. Windows PowerShell ISE besteht aus der Menüleiste, den Windows PowerShell-Registerkarten, der Symbolleiste, den Skriptregisterkarten, dem Skriptbereich, dem Konsolenbereich, der Statusleiste, dem Schieberegler für die Textgröße und der kontextbezogenen Hilfe.

> [!NOTE]
> Ab Windows PowerShell ISE 3.0 wurden der Befehls- und der Ausgabebereich in Form des zentralen Konsolenbereichs zusammengeführt.

## Menüleiste
Die Menüleiste enthält die Menüs **Datei**, **Bearbeiten**, **Ansicht**, **Tools**, **Debuggen**, **Add-Ons** und **Hilfe**. Über die Schaltflächen in den Menüs können Sie Aufgaben erledigen, die im Zusammenhang mit dem Schreiben und Ausführen von Skripts und Ausführen von Befehlen in der Windows PowerShell ISE stehen. Darüber hinaus kann ein [Add-On-Tool](../../core-powershell/ise/The-ISEAddOnTool-Object.md) auf der Menüleiste platziert werden, indem Skripts ausgeführt werden, die das [Windows PowerShell ISE-Skriptobjektmodell](../../core-powershell/ise/The-Windows-PowerShell-ISE-Scripting-Object-Model.md) verwenden.

> [!NOTE]
> In Windows PowerShell ISE 2.0 gab es die Menüs **Tools** und **Add-Ons** nicht.

## Windows PowerShell-Registerkarten
Eine Windows PowerShell-Registerkarte ist die Umgebung, in der ein Windows PowerShell-Skript ausgeführt wird. Sie können neue Windows PowerShell-Registerkarten in der Windows PowerShell ISE öffnen, um auf dem lokalen Computer oder auf Remotecomputern getrennte Umgebungen zu erstellen. Gleichzeitig können maximal acht PowerShell-Registerkarten geöffnet sein.

## Symbolleiste
Die folgenden Schaltflächen befinden sich auf der Symbolleiste.

|Schaltfläche|Funktion|
|----------|------------|
|**Neu**|Öffnet ein neues Skript.|
|**Öffnen Sie**|Öffnet ein vorhandenes Skript oder eine Datei.|
|**Speichern**|Speichert ein Skript oder eine Datei.|
|**Ausschneiden**|Schneidet den ausgewählten Text aus und kopiert ihn in die Zwischenablage.|
|**Kopieren**|Kopiert den ausgewählten Text in die Zwischenablage.|
|**Einfügen**|Fügt den Inhalt der Zwischenablage an der Cursorposition ein.|
|**Deaktivieren Sie Ausgabebereich**|Löscht den gesamten Inhalt aus dem Ausgabebereich.|
|**Rückgängig machen**|Macht die Aktion rückgängig, die zuvor erfolgt ist.|
|**Wiederholen:**|Führt die Aktion aus, die zuvor rückgängig gemacht wurde.|
|**Skript ausführen**|Führt ein Skript aus.|
|**Führen Sie die Markierung**|Führt einen ausgewählten Teil eines Skripts aus.|
|**Ausführung beenden**|Beendet ein Skript, das ausgeführt wird.|
|**Neue Remote-PowerShell-Registerkarte**|Erstellt eine neue PowerShell-Registerkarte, die eine Sitzung auf einem Remotecomputer herstellt. Ein Dialogfeld wird angezeigt und fordert Sie zur Eingabe von Details auf, die zum Herstellen der Remoteverbindung erforderlich sind.|
|**Starten von PowerShell.exe**|Öffnet eine PowerShell-Konsole.|
|**Skript-Bereich nach oben anzeigen**|Verschiebt den Skriptbereich in der Anzeige nach oben.|
|**Skript-Bereich rechts anzeigen**|Verschiebt den Skriptbereich in der Anzeige nach rechts.|
|**Zeigen Sie Skript-Bereich maximiert an**|Maximiert den Skriptbereich.|

## Skriptregisterkarte
Zeigt den Namen des Skripts an, das Sie bearbeiten. Sie können auf eine Skriptregisterkarte klicken, um das Skript auszuwählen, das Sie bearbeiten möchten.

Wenn Sie auf die Skriptregisterkarte zeigen, wird der vollqualifizierte Pfad der Skriptdatei als QuickInfo angezeigt.

## Skriptbereich
Ermöglicht das Erstellen und Ausführen von Skripts. Sie können im Skriptbereich vorhandene Skripts öffnen, bearbeiten und ausführen.

## Ausgabebereich
Zeigt die Ergebnisse der Befehle und Skripts an, die ausgeführt wurden. Sie können den Inhalt des Ausgabebereich auch kopieren und löschen.

## Befehlsbereich
Ermöglicht das Schreiben von Befehlen. Sie können im Befehlsbereich einen Befehl mit einer oder mehreren Zeilen ausführen. Drücken Sie UMSCHALT+EINGABETASTE, um jede Zeile eines mehrzeiligen Befehls einzugeben. Drücken Sie nach der letzten Zeile die EINGABETASTE, um den mehrzeiligen Befehl auszuführen. Die oben im Befehlsbereich angezeigte Aufforderung zeigt den Pfad zum aktuellen Arbeitsverzeichnis.

## Statusleiste
Ermöglicht festzustellen, ob Ihre Befehle und Skripts vollständig ausgeführt wurden. Die Statusleiste befindet sich ganz unten in der Anzeige. Ausgewählte Teile von Fehlermeldungen werden auf der Statusleiste angezeigt.

## Schieberegler für die Textgröße
Erhöht oder verringert die Größe des Texts auf dem Bildschirm.

## Hilfe
Hilfe für Windows PowerShell ISE ist im Web in der TechNet-Bibliothek verfügbar. Sie können die Hilfe öffnen, indem Sie im Menü **Hilfe** auf **Hilfe zu Windows PowerShell ISE** klicken oder die Taste F1 drücken, wobei sich der Cursor allerdings nicht über einem Cmdlet-Namen im Skript- oder Konsolenbereich befinden darf. Im Menü **Hilfe** können Sie auch das Cmdlet „Update-Help“ ausführen und das Befehlsfenster anzeigen. Dieses unterstützt Sie beim Erstellen von Befehlen, indem alle Parameter für ein Cmdlet angezeigt werden und Sie die Parameter in einem benutzerfreundlichen Formular ausfüllen können.

## Weitere Informationen
[Mithilfe von Windows PowerShell ISE](../../core-powershell/ise/Using-the-Windows-PowerShell-ISE.md)




<!--HONumber=Oct16_HO1-->


