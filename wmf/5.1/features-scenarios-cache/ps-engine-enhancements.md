---
title: Verbesserungen des PowerShell-Moduls
author: jasonsh
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 1b35a25312b44d14ec8771be9e17aaa43e270b61

---

# Verbesserungen des PowerShell-Moduls #

Die folgenden Verbesserungen am PowerShell-Kernmodul wurden für PowerShell 5.1 implementiert:


## Leistung ##

In einigen wichtigen Bereichen wurde die Leistung verbessert:

1. Start
2. Das Pipelining zu Cmdlets wie „ForEach-Object“ und „Where-Object“ erfolgt ca. 50 % schneller. 

Einige Beispiele für Verbesserungen (die Ergebnisse variieren je nach Hardware): 

| Szenario | Zeit (ms) in 5.0 | Zeit (ms) in 5.1 |
| -------- | :---------------: | :---------------: |
| `powershell -command "echo 1"` | 900 | 250 |
| Zunächst ist jemals PowerShell aus: `powershell -command "Unknown-Command"` | 30.000 | 13.000 |
| Befehl Analyse Cache erstellt: `powershell -command "Unknown-Command"` | 7000 | 520 |
| <code>1..1000000 &#124; % { }</code> | 1400 | 750 |
  
Eine Änderung im Zusammenhang mit dem Start kann sich möglicherweise auf einige (nicht unterstützte) Szenarien auswirken. PowerShell liest nicht mehr die Dateien des Typs `$pshome\*.ps1xml`. Diese Dateien wurden in C# konvertiert, um einen Teil des Datei- und CPU-Aufwands zum Verarbeiten der XML-Dateien zu vermeiden. Die Dateien sind zur Unterstützung der parallelen Verwendung von Version 2 noch vorhanden. Wenn Sie also den Inhalt der Dateien ändern, hat dies keine Auswirkung auf Version 5, sondern nur auf Version 2. Beachten Sie, dass das Ändern dieser Dateien zu keiner Zeit ein unterstütztes Szenario war.

Eine andere sichtbare Änderung ist, wie PowerShell die exportierten Befehle und andere Informationen für Module zwischenspeichert, die auf einem System installiert sind. Zuvor wurde dieser Cache im Verzeichnis `$env:LOCALAPPDATA\Microsoft\Windows\PowerShell\CommandAnalysis` gespeichert. In WMF 5.1 befindet sich der Cache ausschließlich in der Datei `$env:LOCALAPPDATA\Microsoft\Windows\PowerShell\ModuleAnalysisCache`.
Unter [analysis_cache.md]() finden Sie weitere Details.

Ab Version 5.1 steht PowerShell in verschiedenen Editionen zur Verfügung, die unterschiedliche Featuregruppen und Plattformkompatibilität bieten.



## Fehlerkorrekturen ##

Die folgenden auffälligen Fehler wurden behoben:

### Modul-AutoErmittlung wird vollständig berücksichtigt. `$env:PSModulePath` ###

Die AutoErmittlung von Modulen (d. h. das automatische Laden von Modulen ohne explizites Ausführen von „Import-Module“ beim Aufrufen eines Befehls) wurde in WMF 3 eingeführt. Nach der Einführung suchte PowerShell nach Befehlen in `$PSHome\Modules`, bevor `$env:PSModulePath` verwendet wurde.

In WMF 5.1 ändert sich dieses Verhalten so, dass `$env:PSModulePath` vollständig berücksichtigt wird. Dadurch kann ein vom Benutzer erstelltes Modul, das von PowerShell bereitgestellte Befehle definiert (z. B. `Get-ChildItem`), automatisch geladen werden und den integrierten Befehl ordnungsgemäß überschreiben.

### Umleitung nicht mehr hartcodiert Datei `-Encoding Unicode` ###

In allen früheren Versionen von PowerShell konnte die vom Dateiumleitungsoperator, z. B. `Get-ChildItem > out.txt`, verwendete Dateicodierung nicht gesteuert werden, da PowerShell `-Encoding Unicode` hinzugefügt hat.

Ab WMF 5.1 können Sie jetzt die Dateicodierung der Umleitung durch Festlegen von z. B. `$PSDefaultParameterValues` ändern.

```
$PSDefaultParameterValues["Out-File:Encoding"] = "Ascii"
```

### Korrektur einer Regression beim Zugriff auf Mitglieder `System.Reflection.TypeInfo` ###

Durch eine in WMF 5.0 eingeführte Regression wurde der Zugriff auf Member von `System.Reflection.RuntimeType`, z. B. `[int].ImplementedInterfaces`, unterbrochen.
Dieser Fehler wurde in WMF 5.1 behoben.


### Korrektur einiger Probleme mit COM-Objekten ###

In WMF 5 wurde ein neuer COM-Binder zum Aufrufen von Methoden für COM-Objekte und den Zugriff auf Eigenschaften von COM-Objekten eingeführt.
Dieser neue Binder verbesserte die Leistung erheblich, verursachte jedoch auch einige Fehler, die in WMF 5.1 behoben wurden.

#### Argumentkonvertierungen erfolgten nicht immer ordnungsgemäß ####

Siehe das folgende Beispiel:

```
$obj = New-Object -ComObject WScript.Shell
$obj.SendKeys([char]173)
```

Die „SendKeys“-Methode erwartet eine Zeichenfolge, aber PowerShell konvertierte „char“ nicht in „string“, wodurch die Konvertierung in der „IDispatch::Invoke“-Methode verzögert wurde. Diese Methode verwendet „VariantChangeType“ zum Ausführen der Konvertierung, was bei diesem Beispiel zum Senden der Schlüssel „1“, „7“ und „3 anstelle des erwarteten Schlüssels „Volume.Mute“ geführt hat.

#### Die Verarbeitung aufzählbarer COM-Objekte erfolgte nicht immer ordnungsgemäß ####

PowerShell zählt normalerweise die meisten aufzählbaren Objekte auf. Doch eine in WMF 5 eingeführte Regression verhinderte die Enumeration von COM-Objekten, die „IEnumerable“ implementieren.  Beispiel:

```
function Get-COMDictionary
{
    $d = New-Object -ComObject Scripting.Dictionary
    $d.Add('a', 2)
    $d.Add('b', 2)
    return $d
}

$x = Get-COMDictionary
```

Im obigen Beispiel schreibt WMF 5.0 „Scripting.Dictionary“ fälschlicherweise in die Pipeline, anstatt die Schlüssel-/Wert-Paare aufzuzählen.


### `[ordered]` wurde nicht innerhalb von Klassen zulässig. ###

In WMF 5 wurden Klassen mit einer in Klassen verwendeten Überprüfung von Typliteralen eingeführt.  `[ordered]` sieht wie ein Typ-Literal ist jedoch kein echter .NET-Typ.  WMF 5 meldete fälschlicherweise einen Fehler für `[ordered]` innerhalb einer Klasse:

```
class CThing
{
    [object] foo($i)
    {
        [ordered]@{ Thing = $i }
    }
}
```


### Hilfe zu Themen mit mehreren Versionen funktioniert nicht ###

Wenn Sie vor WMF 5.1 über mehrere Versionen eines installierten Modus verfügt haben und es für alle nur ein Hilfethema gab, z. B. „about_PSReadline“, gab `help about_PSReadline` mehrere Themen zurück, ohne dass es eine erkennbare Möglichkeit gab, die tatsächliche Hilfe anzuzeigen.

In WMF 5.1 wurde dieser Fehler behoben, sodass nur die Hilfe für die neueste Version des Themas zurückgegeben wird.

„Get-Help“ bietet keine Möglichkeit anzugeben, für welche Version Sie Hilfe wünschen. Die Problemumgebung besteht darin, zum Verzeichnis „Modules“ zu navigieren und die Hilfe direkt mit einem Tool wie Ihrem bevorzugten Editor anzuzeigen. 



<!--HONumber=Oct16_HO1-->


