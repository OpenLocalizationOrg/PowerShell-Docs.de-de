---
title: Verwenden von statischen Klassen und Methoden
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 418ad766-afa6-4b8c-9a44-471889af7fd9
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 28bc665c3ffb1b74a2ff922584c31a8657842a0f

---

# Verwenden von statischen Klassen und Methoden
Nicht alle .NET Framework-Klassen können mit **New-Object** erstellt werden. Wenn Sie beispielsweise versuchen, ein **System.Environment**- oder ein **System.Math**-Objekt mit **New-Object** zu erstellen, erhalten Sie die folgenden Fehlermeldungen:

```
PS> New-Object System.Environment
New-Object : Constructor not found. Cannot find an appropriate constructor for
type System.Environment.
At line:1 char:11
+ New-Object  <<<< System.Environment
PS> New-Object System.Math
New-Object : Constructor not found. Cannot find an appropriate constructor for
type System.Math.
At line:1 char:11
+ New-Object  <<<< System.Math
```

Diese Fehler treten auf, weil es keine Möglichkeit gibt, ein neues Objekt aus diesen Klassen zu erstellen. Diese Klassen sind Verweisbibliotheken von Methoden und Eigenschaften, deren Status nicht geändert werden. Sie müssen sie nicht erstellen, Sie verwenden sie einfach. Klassen und Methoden wie diese werden als *statische Klassen* bezeichnet, weil sie nicht erstellt, gelöscht oder geändert werden. Um dies zu verdeutlichen, folgen einige Beispiele, in denen statische Klassen verwendet werden.

### Abrufen von Umgebungsdaten mit „System.Environment“
Normalerweise besteht der erste Schritt beim Arbeiten mit einem Objekt in Windows PowerShell darin, über „Get-Member“ zu ermitteln, welche Member das Objekt enthält. Bei statischen Klassen ist die Vorgehensweise ein wenig anders, weil die jeweilige tatsächliche Klasse kein Objekt ist.

#### Verweisen auf die statische „System.Environment“-Klasse
Sie können auf eine statische Klasse verweisen, indem Sie den Klassennamen in rechteckige Klammern setzen. Beispielsweise können Sie auf **System.Environment** verweisen, indem Sie den Namen in Klammern eingeben. Wenn Sie dies tun, werden einige allgemeine Informationen angezeigt:

```
PS> [System.Environment]

IsPublic IsSerial Name                                     BaseType
-------- -------- ----                                     --------
True     False    Environment                              System.Object
```

> [!NOTE]
> Wie bereits erwähnt wurde, setzt Windows PowerShell automatisch **System.** vor die Typnamen, wenn Sie **New-Object** verwenden. Das gleiche passiert, wenn Sie einen in Klammern stehenden Namen verwenden, d. h., Sie können **\[System.Environment]** als **\[Environment]** angeben.

Die **System.Environment**-Klasse enthält allgemeine Informationen über die Arbeitsumgebung des aktuellen Prozesses, der „powershell.exe“ ist, wenn Sie in Windows PowerShell arbeiten.

Wenn Sie versuchen, die Details dieser Klasse anzeigen **\[System.Environment] | Get-Member**, der Objekttyp laut **System.RuntimeType** , nicht **System.Environment**:

```
PS> [System.Environment] | Get-Member

   TypeName: System.RuntimeType
```

Wenn Sie statische Member mit „Get-Member“ anzeigen möchten, geben Sie den **Static**-Parameter an:

```
PS> [System.Environment] | Get-Member -Static

   TypeName: System.Environment

Name                       MemberType Definition
----                       ---------- ----------
Equals                     Method     static System.Boolean Equals(Object ob...
Exit                       Method     static System.Void Exit(Int32 exitCode)
...
CommandLine                Property   static System.String CommandLine {get;}
CurrentDirectory           Property   static System.String CurrentDirectory ...
ExitCode                   Property   static System.Int32 ExitCode {get;set;}
HasShutdownStarted         Property   static System.Boolean HasShutdownStart...
MachineName                Property   static System.String MachineName {get;}
NewLine                    Property   static System.String NewLine {get;}
OSVersion                  Property   static System.OperatingSystem OSVersio...
ProcessorCount             Property   static System.Int32 ProcessorCount {get;}
StackTrace                 Property   static System.String StackTrace {get;}
SystemDirectory            Property   static System.String SystemDirectory {...
TickCount                  Property   static System.Int32 TickCount {get;}
UserDomainName             Property   static System.String UserDomainName {g...
UserInteractive            Property   static System.Boolean UserInteractive ...
UserName                   Property   static System.String UserName {get;}
Version                    Property   static System.Version Version {get;}
WorkingSet                 Property   static System.Int64 WorkingSet {get;}
TickCount                               ExitCode
```

Nun können Sie anzuzeigende Eigenschaften aus „System.Environment“ auswählen.

#### Anzeigen von statischen Eigenschaften von „System.Environment“
Die Eigenschaften von „System.Environment“ sind ebenfalls statisch und müssen auf andere Weise angegeben werden als normale Eigenschaften. Es wird **::** verwendet, um für Windows PowerShell anzugeben, dass mit einer statischen Methode oder Eigenschaft gearbeitet werden soll. Um den Befehl anzuzeigen, mit dem Windows PowerShell gestartet wurde, wird die **CommandLine**-Eigenschaft geprüft, indem Folgendes eingegeben wird:

```
PS> [System.Environment]::Commandline
"C:\Program Files\Windows PowerShell\v1.0\powershell.exe"
```

Um die Version des Betriebssystems zu überprüfen, zeigen Sie die „OSVersion“-Eigenschaft durch folgende Eingabe an:

```
PS> [System.Environment]::OSVersion

           Platform ServicePack         Version             VersionString
           -------- -----------         -------             -------------
            Win32NT Service Pack 2      5.1.2600.131072     Microsoft Windows...
```

Durch Anzeigen der **HasShutdownStarted**-Eigenschaft können Sie überprüfen, ob der Computer momentan heruntergefahren wird:

```
PS> [System.Environment]::HasShutdownStarted
False
```

### Ausführen von mathematischen Funktionen mit „System.Math“
Mit der statischen „System.Math“-Klasse können einige mathematische Operationen ausgeführt werden. Die wichtigen Member von **System.Math** sind größtenteils Methoden, die mit **Get-Member** angezeigt werden können.

> [!NOTE]
> „System.Math“ hat mehrere Methoden mit demselben Namen, diese werden aber durch die Typen ihrer Parameter unterschieden.

Geben Sie den folgenden Befehl aus, um die Methoden der **System.Math**-Klasse aufzulisten.

```
PS> [System.Math] | Get-Member -Static -MemberType Methods

   TypeName: System.Math

Name            MemberType Definition
----            ---------- ----------
Abs             Method     static System.Single Abs(Single value), static Sy...
Acos            Method     static System.Double Acos(Double d)
Asin            Method     static System.Double Asin(Double d)
Atan            Method     static System.Double Atan(Double d)
Atan2           Method     static System.Double Atan2(Double y, Double x)
BigMul          Method     static System.Int64 BigMul(Int32 a, Int32 b)
Ceiling         Method     static System.Double Ceiling(Double a), static Sy...
Cos             Method     static System.Double Cos(Double d)
Cosh            Method     static System.Double Cosh(Double value)
DivRem          Method     static System.Int32 DivRem(Int32 a, Int32 b, Int3...
Equals          Method     static System.Boolean Equals(Object objA, Object ...
Exp             Method     static System.Double Exp(Double d)
Floor           Method     static System.Double Floor(Double d), static Syst...
IEEERemainder   Method     static System.Double IEEERemainder(Double x, Doub...
Log             Method     static System.Double Log(Double d), static System...
Log10           Method     static System.Double Log10(Double d)
Max             Method     static System.SByte Max(SByte val1, SByte val2), ...
Min             Method     static System.SByte Min(SByte val1, SByte val2), ...
Pow             Method     static System.Double Pow(Double x, Double y)
ReferenceEquals Method     static System.Boolean ReferenceEquals(Object objA...
Round           Method     static System.Double Round(Double a), static Syst...
Sign            Method     static System.Int32 Sign(SByte value), static Sys...
Sin             Method     static System.Double Sin(Double a)
Sinh            Method     static System.Double Sinh(Double value)
Sqrt            Method     static System.Double Sqrt(Double d)
Tan             Method     static System.Double Tan(Double a)
Tanh            Method     static System.Double Tanh(Double value)
Truncate        Method     static System.Decimal Truncate(Decimal d), static...
```

Dadurch werden verschiedene mathematische Methoden angezeigt. Es folgt eine Liste von Befehlen, anhand denen veranschaulicht wird, wie einige der üblichen Methoden funktionieren:

```
PS> [System.Math]::Sqrt(9)
3
PS> [System.Math]::Pow(2,3)
8
PS> [System.Math]::Floor(3.3)
3
PS> [System.Math]::Floor(-3.3)
-4
PS> [System.Math]::Ceiling(3.3)
4
PS> [System.Math]::Ceiling(-3.3)
-3
PS> [System.Math]::Max(2,7)
7
PS> [System.Math]::Min(2,7)
2
PS> [System.Math]::Truncate(9.3)
9
PS> [System.Math]::Truncate(-9.3)
-9
```




<!--HONumber=Oct16_HO1-->


