---
title: Verbesserungen am PowerShell-Modul in WMF 5.1 (Vorschau)
ms.date: 2016-07-13
keywords: PowerShell, DSC, WMF
description: 
ms.topic: article
author: keithb
manager: dongill
ms.prod: powershell
ms.technology: WMF
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: bb7efc55b1c948c349aa778b700e5cb1277b9762

---

#Verbesserungen am PowerShell-Modul

Die folgenden Verbesserungen am PowerShell-Kernmodul wurden in WMF 5.1 implementiert:


## Leistung ##

In einigen wichtigen Bereichen wurde die Leistung verbessert:

- Start
- Das Pipelining zu Cmdlets wie „ForEach-Object“ und „Where-Object“ erfolgt ca. 50 % schneller. 

Einige Beispiele für Verbesserungen (die Ergebnisse variieren je nach Hardware): 

| Szenario | Zeit (ms) in 5.0 | Zeit (ms) in 5.1 |
| -------- | :---------------: | :---------------: |
| `powershell -command "echo 1"` | 900 | 250 |
| Zunächst ist jemals PowerShell aus: `powershell -command "Unknown-Command"` | 30.000 | 13.000 |
| Befehl Analyse Cache erstellt: `powershell -command "Unknown-Command"` | 7000 | 520 |
| <code>1..1000000 &#124; % { }</code> | 1400 | 750 |
  
> Hinweis: Eine Änderung im Zusammenhang mit dem Start kann sich möglicherweise auf einige (nicht unterstützte) Szenarien auswirken. 
> PowerShell liest nicht mehr die Dateien des Typs `$pshome\*.ps1xml`. Diese Dateien wurden in C# konvertiert, um den Datei- und CPU-Overhead beim Verarbeiten der XML-Dateien zu reduzieren. 
Die Dateien sind zur Unterstützung der parallelen Verwendung von Version 2 noch vorhanden. Wenn Sie also den Inhalt der Dateien ändern, hat dies keine Auswirkung auf Version 5, sondern nur auf Version 2. 
Beachten Sie, dass das Ändern dieser Dateien zu keiner Zeit ein unterstütztes Szenario war.

Eine andere sichtbare Änderung ist, wie PowerShell die exportierten Befehle und andere Informationen für Module zwischenspeichert, die auf einem System installiert sind. Zuvor wurde dieser Cache im Verzeichnis `$env:LOCALAPPDATA\Microsoft\Windows\PowerShell\CommandAnalysis` gespeichert. In WMF 5.1 befindet sich der Cache ausschließlich in der Datei `$env:LOCALAPPDATA\Microsoft\Windows\PowerShell\ModuleAnalysisCache`.
Unter [Die Datei „ModuleAnalysisCache“](scenarios-features.md#module-analysis-cache) finden Sie weitere Details.



<!--HONumber=Oct16_HO1-->


