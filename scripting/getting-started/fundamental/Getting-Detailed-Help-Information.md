---
title: "Abrufen von ausführlichen Hilfeinformationen"
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 6fb4daf7-8607-4a3e-b692-f77631adc1b9
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 423be0f76788fa8f45356ca8446bad6754d38e1a

---

# Abrufen von ausführlichen Hilfeinformationen
Windows PowerShell umfasst ausführliche Hilfethemen, in denen die Konzepte von Windows PowerShell und die Windows PowerShell-Sprache erläutert werden. Außerdem gibt es Hilfethemen für jedes Cmdlet und jeden Anbieter sowie Hilfethemen für viele Funktionen und Skripts.

Sie können diese Hilfethemen über die Eingabeaufforderung anzeigen, oder Sie können die zuletzt aktualisierten Versionen dieser Themen in der Microsoft TechNet Library anzeigen. Viele Programme, von denen Windows PowerShell gehostet wird, z.B. Windows PowerShell Integrated Scripting Environment, stellen zusätzliche Hilfefunktionen bereit, etwa kontextbezogene Hilfe und kompilierte Hilfedateien (CHM).

## Abrufen von Hilfe für Cmdlets
Wenn Sie Hilfe zu Windows PowerShell-Cmdlets abrufen möchten, verwenden Sie das Cmdlet [Get-Help [m2]](https://technet.microsoft.com/en-us/library/2d7fe1b4-0025-4580-a911-d81922dd6cd2). Geben Sie beispielsweise Folgendes ein, um Hilfe für das Cmdlet [Get-ChildItem [m2]](https://technet.microsoft.com/en-us/library/4b270d63-c995-45b8-b5b4-3f8887efbfcc) abzurufen:

```
get-help get-childitem
```

oder

```
get-childitem -?
```

Sie können sogar Hilfe zu dem Cmdlet „Get-Help“ abrufen. Beispiel:

```
get-help get-help
```

Um eine Liste aller Cmdlet-Hilfethemen in der Sitzung abzurufen, geben Sie Folgendes ein:

```
get-help -category cmdlet
```

Wenn Sie nur jeweils eine Seite jedes Hilfethemas anzeigen möchten, verwenden Sie die Funktion **help** oder deren Alias **man**. Wenn Sie beispielsweise Hilfe für das Cmdlet „Get-ChildItem“ anzeigen möchten, geben Sie Folgendes ein:

```
man get-childitem
```

oder

```
help get-childitem
```

Um detaillierte Informationen zu einem Cmdlet, zu einer Funktion oder zu einem Skript samt Beschreibungen der jeweiligen Parameter sowie Beispielen für deren Verwendung anzuzeigen, verwenden Sie den *Detailed*-Parameter des Cmdlets „Get-Help“. Geben Sie beispielsweise Folgendes ein, um ausführliche Informationen zum Cmdlet „Get-ChildItem“ abzurufen:

```
get-help get-childitem -detailed
```

Wenn Sie den gesamten Inhalt eines Hilfethemas anzeigen möchten, verwenden Sie den *Full*-Parameter des Cmdlets „Get-Help“. Soll beispielsweise der gesamte Inhalt des Hilfethemas für das Cmdlet „Get-ChildItem“ angezeigt werden, geben Sie Folgendes ein:

```
get-help get-childitem -full
```

Um ausführliche Hilfe zu den Parametern eines Cmdlets abzurufen, verwenden Sie den *Parameter*-Parameter des Cmdlets „Get-Help“. Soll beispielsweise die ausführliche Hilfe zu allen Parametern des Cmdlets „Get-ChildItem“ abgerufen werden, geben Sie Folgendes ein:

```
get-help get-childitem -parameter *
```

Um nur die Beispiele anzuzeigen, die es in einem Hilfethema gibt, verwenden Sie den *Example*-Parameter von „Get-Help“. Sollen beispielsweise nur die Beispiele im Hilfethema für das Cmdlet „Get-ChildItem“ angezeigt werden, geben Sie Folgendes ein:

```
get-help get-childitem -examples
```

Informationen dazu, wie Sie Hilfethemen für die Cmdlets schreiben, die Sie erstellen, finden Sie im Thema „How to Write Cmdlet Help“ in MSDN.

## Abrufen von konzeptioneller Hilfe
Das Cmdlet „Get-Help“ zeigt auch Informationen zu konzeptionellen Themen in Windows PowerShell an, einschließlich Themen zur Windows PowerShell-Sprache. Konzeptionelle Hilfethemen beginnen mit dem Präfix „about_“, z.B. „about_line_editing“. (Der Name eines konzeptionellen Themas muss in Englisch eingegeben werden, auch in nicht englischsprachigen Versionen von Windows PowerShell.)

Um eine Liste der konzeptionellen Themen anzuzeigen, geben Sie Folgendes ein:

```
get-help about_*
```

Um ein bestimmtes Hilfethema anzuzeigen, geben Sie den Namen des Themas ein, zum Beispiel:

```
get-help about_command_syntax
```

Die Parameter von „Get-Help“, etwa *Detailed*, *Parameter* und *Examples*, wirken sich nicht auf die Anzeige von konzeptionellen Hilfethemen aus.

## Abrufen von Hilfe zu Anbietern
Das Cmdlet „Get-Help“ zeigt Informationen zu Windows PowerShell-Anbietern an. Um Hilfe zu einem Anbieter (Provider) zu abzurufen, geben Sie „Get-Help“ gefolgt vom Anbieternamen ein. Wenn Sie z. B. Hilfe zum Registrierungsanbieter abrufen möchten, geben Sie Folgendes ein:

```
get-help registry
```

Um eine Liste aller Anbieter-Hilfethemen in der Sitzung abzurufen, geben Sie Folgendes ein:

```
get-help -category provider
```

Die Parameter von „Get-Help“, etwa *Detailed*, *Parameter* und *Examples*, wirken sich nicht auf die Anzeige von Anbieter-Hilfethemen aus.

## Abrufen von Hilfe zu Skripts und Funktionen
Viele Skripts und Funktionen in Windows PowerShell haben Hilfethemen. Verwenden Sie das Cmdlet „Get-Help“, um die Hilfethemen für Skripts und Funktionen anzuzeigen.

Wenn Sie die Hilfe für eine Funktion anzeigen möchten, geben Sie „get-help“ und dahinter den Namen der Funktion ein. Soll beispielsweise Hilfe für die Funktion „Disable-PSRemoting“ abgerufen werden, geben Sie Folgendes ein:

```
get-help disable-psremoting
```

Um die Hilfe für ein Skript anzuzeigen, geben Sie den vollqualifizierten Pfad zur Skriptdatei ein. Befindet sich das Skript in einem Pfad, der in der Umgebungsvariablen „Path“ aufgelistet ist, müssen Sie den Pfad nicht im Befehl angeben.

Beispielsweise geben Sie, wenn Sie ein Skript namens "TestScript.ps1" in Ihrem Verzeichnis C:\\PS-Test verfügen, um das Hilfethema für das Skript aufzurufen:

```
get-help c:\ps-test\TestScript.ps1
```

Die Parameter, die für die Anzeige von Cmdlet-Hilfe konzipiert wurden, etwa *Detailed*, *Full*, *Examples* und *Parameter*, funktionieren auch für Hilfe zu Skripts und Funktionen. Jedoch wenn Sie alle Hilfe anzeigen, eingeben "Get-Help \ *", die Hilfe für Funktionen und Skripts nicht angezeigt.

Informationen über das Schreiben von Hilfethemen für Ihre Funktionen und Skripts finden Sie unter [about_Functions [m2]](https://technet.microsoft.com/en-us/library/61d40692-5300-4de9-a9b5-bae31815e105), [about_Scripts](https://technet.microsoft.com/en-us/library/7dc08334-dcfe-450b-b949-0554855623af) und [about_Comment_Based_Help](https://technet.microsoft.com/en-us/library/99a81ccc-21a0-49ec-a1b3-9efe2b4c0bbf).

## Abrufen von Hilfe aus dem Internet
Wenn Sie eine Verbindung mit dem Internet haben, ist eine der besten Möglichkeiten zum Abrufen von Hilfe, die Hilfethemen online anzuzeigen. Da online verfügbare Themen einfach zu aktualisieren sind, stellen sie wahrscheinlich die aktuellsten Inhalte bereit.

Um Hilfe aus dem Internet abzurufen, verwenden Sie den *Online*-Parameter des Cmdlets „Get-Help“. Der *Online*-Parameter des Cmdlets „Get-Help“ funktioniert nur für Hilfe zu Cmdlets, Funktionen und Skripts. Der *Online*-Parameter kann nicht mit konzeptionellen Themen (About) oder Anbieter-Hilfethemen verwendet werden. Außerdem funktioniert dieser Parameter, da er optional ist, nicht für alle Hilfethemen zu Cmdlets, Funktionen oder Skripts.

Allerdings sind alle Hilfethemen, die zu Windows PowerShell gehören, einschließlich Anbieter-Hilfe und konzeptioneller Hilfethemen (About), online im [Windows PowerShell](http://go.microsoft.com/fwlink/?LinkID=107116)-Abschnitt der Microsoft TechNet-Bibliothek verfügbar.

Wenn Sie den *Online*-Parameter des Cmdlets „Get-Help“ verwenden möchten, verwenden Sie das folgende Befehlsformat.

```
get-help <command-name> -online
```

Möchten Sie beispielsweise die Onlineversion des Hilfethemas zum Cmdlet „Get-ChildItem“ abrufen, geben Sie Folgendes ein:

```
get-help get-childitem -online
```

Ist eine Onlineversion des Hilfethemas verfügbar, wird sie in Ihrem Standardbrowser geöffnet.

Wird Onlinehilfe für ein Hilfethema unterstützt, können Sie auch die Internetadresse (URL) des Hilfethemas anzeigen. Die Internetadresse wird im Abschnitt „Verwandte Links“ eines Hilfethemas angezeigt.

Wenn Sie beispielsweise die URL für die Onlineversion des Cmdlets „Add-Computer“ anzeigen möchten, geben Sie Folgendes ein:

```
get-help add-computer
```

Die erste Zeile des Abschnitts „Verwandte Links“ des Themas ist nachstehend dargestellt.

```
Online version: http://go.microsoft.com/fwlink/?LinkID=135194
```

Informationen dazu, wie Sie die Onlineunterstützung für Ihre Hilfethemen bereitstellen, finden Sie unter [about_Comment_Based_Help](https://technet.microsoft.com/en-us/library/99a81ccc-21a0-49ec-a1b3-9efe2b4c0bbf) sowie unter „How to Write Cmdlet Help“ ([http://go.microsoft.com/fwlink/?LinkID=123415](http://go.microsoft.com/fwlink/?LinkID=123415)) in der MSDN-Bibliothek (Microsoft Developer Network).

## Weitere Informationen
[about_Functions [m2]](https://technet.microsoft.com/en-us/library/61d40692-5300-4de9-a9b5-bae31815e105)
[about_Scripts](https://technet.microsoft.com/en-us/library/7dc08334-dcfe-450b-b949-0554855623af)
[about_Comment_Based_Help](https://technet.microsoft.com/en-us/library/99a81ccc-21a0-49ec-a1b3-9efe2b4c0bbf)
[Get-Help [m2]](https://technet.microsoft.com/en-us/library/2d7fe1b4-0025-4580-a911-d81922dd6cd2)




<!--HONumber=Oct16_HO1-->


