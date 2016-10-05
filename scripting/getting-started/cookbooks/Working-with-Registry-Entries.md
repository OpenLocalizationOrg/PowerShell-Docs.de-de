---
title: "Arbeiten mit Registrierungseinträge"
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: fd254570-27ac-4cc9-81d4-011afd29b7dc
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: cdc7f45c9fa8a6bf748a52b460a1ac190d283971

---

# Arbeiten mit Registrierungseinträge
Da Registrierungseinträge Eigenschaften von Schlüsseln sind und daher nicht direkt gesucht werden können, muss für die Arbeit mit diesen ein etwas anderer Ansatz gewählt werden.

### Auflisten von Registrierungseinträgen
Es gibt viele verschiedene Möglichkeiten zum Untersuchen von Registrierungseinträgen. Am einfachsten ist es, die Eigenschaftennamen mit einem Schlüssel zu verknüpfen. Um beispielsweise die Namen der Einträge in der Registrierung finden Sie unter **HKEY_LOCAL_MACHINE\\Software\\Microsoft\\Windows\\CurrentVersion**, verwenden **Get-Item**. Registrierungsschlüssel verfügen über eine Eigenschaft mit dem generischen Namen „Property“, die eine Liste der Registrierungseinträge im Schlüssel ist. Der folgende Befehl wählt die Eigenschaft „Property“ aus und erweitert die Elemente so, dass sie in einer Liste angezeigt werden:

```
PS> Get-Item -Path Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion | Select-Object -ExpandProperty Property
DevicePath
MediaPathUnexpanded
ProgramFilesDir
CommonFilesDir
ProductId
```

Um die Registrierungseinträge in einer besser lesbaren Form anzuzeigen, verwenden Sie **Get-ItemProperty**:

```
PS> Get-ItemProperty -Path Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion

PSPath              : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SO
                      FTWARE\Microsoft\Windows\CurrentVersion
PSParentPath        : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SO
                      FTWARE\Microsoft\Windows
PSChildName         : CurrentVersion
PSDrive             : HKLM
PSProvider          : Microsoft.PowerShell.Core\Registry
DevicePath          : C:\WINDOWS\inf
MediaPathUnexpanded : C:\WINDOWS\Media
ProgramFilesDir     : C:\Program Files
CommonFilesDir      : C:\Program Files\Common Files
ProductId           : 76487-338-1167776-22465
WallPaperDir        : C:\WINDOWS\Web\Wallpaper
MediaPath           : C:\WINDOWS\Media
ProgramFilesPath    : C:\Program Files
PF_AccessoriesName  : Accessories
(default)           :
```

Die Windows PowerShell-bezogenen Eigenschaften für den Schüssel verfügen alle über das Präfix „PS“, z.B. **PSPath**, **PSParentPath**, **PSChildName** und **PSProvider**.

Sie können die Notation „**.**“ zum Verweisen auf den aktuellen Speicherort verwenden. Sie können **Set-Location** verwenden, um zuerst in den Registrierungscontainer **CurrentVersion** zu wechseln:

```
Set-Location -Path Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion
```

Alternativ dazu können Sie das integrierte HKLM PSDrive mit **Set-Location** verwenden:

```
Set-Location -Path hklm:\SOFTWARE\Microsoft\Windows\CurrentVersion
```

Anschließend können Sie die Notation „**.**“ für den aktuellen Speicherort verwenden, um die Eigenschaften aufzulisten, ohne einen vollständigen Pfad anzugeben:

```
PS> Get-ItemProperty -Path .
...
DevicePath          : C:\WINDOWS\inf
MediaPathUnexpanded : C:\WINDOWS\Media
ProgramFilesDir     : C:\Program Files
...
```

Pfad-Erweiterung funktioniert gleich wie im Dateisystem, und von diesem Speicherort kann die **ItemProperty** für **HKLM:\\SOFTWARE\\Microsoft\\Windows\\Help** mit **Get-ItemProperty-Pfad... \\Help**.

### Abrufen eines einzelnen Registrierungseintrags
Wenn Sie einen bestimmten Eintrag in einem Registrierungsschlüssel abrufen möchten, stehen verschiedene Möglichkeiten zur Verfügung. Dieses Beispiel sucht nach den Wert der **DevicePath** in **HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion**.

Verwenden Sie mit **Get-ItemProperty** den Parameter **Path**, um den Namen des Schlüssels anzugeben, und den Parameter **Name**, um den Namen des Eintrags **DevicePath** anzugeben.

```
PS> Get-ItemProperty -Path HKLM:\Software\Microsoft\Windows\CurrentVersion -Name DevicePath

PSPath       : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\Software\
               Microsoft\Windows\CurrentVersion
PSParentPath : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\Software\
               Microsoft\Windows
PSChildName  : CurrentVersion
PSDrive      : HKLM
PSProvider   : Microsoft.PowerShell.Core\Registry
DevicePath   : C:\WINDOWS\inf
```

Dieser Befehl gibt die standardmäßigen Windows PowerShell-Eigenschaften sowie die Eigenschaft **DevicePath** aus.

> [!NOTE]
> Obwohl **Get-ItemProperty** die Parameter **Filter**, **Include** und **Exclude** aufweist, können diese nicht zum Filtern nach dem Namen der Eigenschaft verwendet werden. Diese Parameter verweisen auf Registrierungsschlüssel (das sind Elementpfade und keine Registrierungseinträge), die Elementeigenschaften sind.

Eine weitere Option ist die Verwendung des Befehlszeilentools „Reg.exe“. Um Hilfe zu „reg.exe“ zu erhalten, geben Sie **reg.exe /?** an einer Eingabeaufforderung ein. an einer Eingabeaufforderung. Mithilfe von „reg.exe“ finden Sie den Eintrag „DevicePath“, wie im folgenden Befehl gezeigt:

```
PS> reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion /v DevicePath

! REG.EXE VERSION 3.0

HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion
    DevicePath  REG_EXPAND_SZ   %SystemRoot%\inf
```

Sie können das Objekt **WshShell COM** auch zum Suchen bestimmter Registrierungseinträge verwenden, obwohl diese Methode mit großen Binärdaten oder mit Registrierungseintragsnamen, die Zeichen wie „\“ enthalten, nicht funktioniert. Fügen Sie den Namen der Eigenschaft mit einem \-Trennzeichen zum Elementpfad hinzu:

```
PS> (New-Object -ComObject WScript.Shell).RegRead("HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\DevicePath")
%SystemRoot%\inf
```

### Erstellen neuer Registrierungseinträge
Um einen neuen Eintrag namens „PowerShellPath“ zum Schlüssel **CurrentVersion** hinzuzufügen, verwenden Sie **New-ItemProperty** mit dem Pfad zum Schüssel, dem Eintragsnamen und dem Wert des Eintrags. In diesem Beispiel wird der Wert der Windows PowerShell-Variablen **$PSHome** verwendet, in der der Pfad zum Installationsverzeichnis für Windows PowerShell gespeichert wird.

Sie können dem Schlüssel mit Hilfe des folgenden Befehls den neuen Eintrag hinzufügen, und der Befehl gibt auch Informationen zu dem neuen Eintrag zurück:

```
PS> New-ItemProperty -Path HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion -Name PowerShellPath -PropertyType String -Value $PSHome

PSPath         : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SOFTWAR
                 E\Microsoft\Windows\CurrentVersion
PSParentPath   : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SOFTWAR
                 E\Microsoft\Windows
PSChildName    : CurrentVersion
PSDrive        : HKLM
PSProvider     : Microsoft.PowerShell.Core\Registry
PowerShellPath : C:\Program Files\Windows PowerShell\v1.0
```

**PropertyType** muss der Namen eines **Microsoft.Win32.RegistryValueKind**-Aufzählungselements aus der folgenden Tabelle sein:

|PropertyType-Wert|Bedeutung|
|----------------------|-----------|
|Binär|Binärdaten|
|DWord|Eine Zahl, die eine gültige UInt32 ist|
|ExpandString|Eine Zeichenfolge, die Umgebungsvariablen enthalten kann, die dynamisch erweitert werden|
|MultiString|Eine mehrzeilige Zeichenfolge|
|Zeichenfolge|Ein beliebiger Zeichenfolgenwert|
|QWord|8 Byte Binärdaten|

> [!NOTE]
> Sie können einen Registrierungseintrag zu mehreren Speicherorten hinzufügen, indem Sie ein Array von Werten für den Parameter **Path** angeben:

```
New-ItemProperty -Path HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion, HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion -Name PowerShellPath -PropertyType String -Value $PSHome
```

Sie können auch einen bereits vorhandenen Registrierungseintragswert überschreiben, indem Sie den Parameter **Force** an einen beliebigen **New-ItemProperty**-Befehl anfügen.

### Umbenennen von Registrierungseinträgen
Um den Eintrag **PowerShellPath** in „PSHome“ umzubenennen, verwenden Sie **Rename-ItemProperty**:

```
Rename-ItemProperty -Path HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion -Name PowerShellPath -NewName PSHome
```

Um den umbenannten Wert anzuzeigen, fügen Sie dem Befehl den Parameter **PassThru** hinzu.

```
Rename-ItemProperty -Path HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion -Name PowerShellPath -NewName PSHome -passthru
```

### Löschen von Registrierungseinträgen
Um die Registrierungseinträge „PSHome“ und „PowerShellPath“ zu löschen, verwenden Sie **Remove-ItemProperty**:

```
Remove-ItemProperty -Path HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion -Name PSHome
Remove-ItemProperty -Path HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion -Name PowerShellPath
```




<!--HONumber=Oct16_HO1-->


