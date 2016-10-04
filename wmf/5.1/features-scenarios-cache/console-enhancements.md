---
title: Verbesserungen an der Konsolenumgebung
author: jasonsh
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: e6653a02421e3aec3910a70c64f7cf7cecd696ab

---

>Hinweis: Vorschlag für einen aussagekräftigen Namen und eine Kurzbeschreibung bereitstellen

## Verbesserungen an der PowerShell-Konsole

Die folgenden Änderungen an „powershell.exe“ sind zum Verbessern der Konsolenumgebung erfolgt:

1. Unterstützung von VT100

Windows 10 unterstützt nun [VT100-Escapesequenzen](https://msdn.microsoft.com/en-us/library/windows/desktop/mt638032(v=vs.85).aspx).
Bei der Berechnung von Tabellenbreiten ignoriert PowerShell bestimmte VT100-Escapesequenzen für die Formatierung.

PowerShell bietet nun auch eine neue API, die in Formatierungscode verwendet werden kann, um festzustellen, ob VT100 unterstützt wird.  Beispiel:

```
if ($host.UI.SupportsVirtualTerminal)
{
    $esc = [char]0x1b
    "A yellow ${esc}[93mhello${esc}[0m"
}
else
{
    "A default hello"
}
```
Es folgt ein vollständiges [Beispiel](https://gist.github.com/lzybkr/dcb973dccd54900b67783c48083c28f7), das zum Hervorheben von Übereinstimmungen aus „Select-String“ verwendet werden kann.
Speichern Sie das Beispiel in einer Datei namens `MatchInfo.format.ps1xml`. Verwenden Sie sie in Ihrem Profil oder an anderer Stelle, und führen Sie `Update-FormatData -Prepend MatchInfo.format.ps1xml` aus.

Beachten Sie, dass VT100-Escapesequenzen erst ab dem Anniversary-Update für Windows 10 unterstützt werden. Von vorherigen Systemen werden sie nicht unterstützt.   

2. Unterstützung des vi-Modus in PSReadline

[PSReadline](https://github.com/lzybkr/PSReadLine) bietet nun Unterstützung den für vi-Modus. Führen Sie zur Verwendung des vi-Modus `Set-PSReadline -EditMode vi` aus.

3. Umgeleitetes stdin mit interaktiver Eingabe 

In früheren Versionen war das Starten von PowerShell mit `powershell -File -` erforderlich, wenn stdin umgeleitet wurde und Sie Befehle interaktiv eingeben wollten.

Ab WMF 5.1 ist diese nur schwer auffindbare Option nicht mehr erforderlich. Sie können PowerShell ohne Optionen starten, z. B. `powershell`.

Beachten Sie, dass PSReadline derzeit umgeleitetes stdin nicht unterstützt und dass die integrierte Bearbeitungsumgebung für die Befehlszeile mit umgeleitetem sdtdin sehr eingeschränkt ist. Beispielsweise funktionieren die Pfeiltasten nicht.  In einer künftigen Version von PSReadline soll dieses Problem behoben werden.   


<!--HONumber=Oct16_HO1-->


