---
title: "Schreiben von Hilfe für DSC-Konfigurationen"
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: becacd2dcbc6fd0edd9154a45342edc5c536935b

---

# Schreiben von Hilfe für DSC-Konfigurationen

>Gilt für: Windows PowerShell 5.0

Sie können die kommentarbasierte Hilfe in DSC-Konfigurationen verwenden. Benutzer können auf die Hilfe zugreifen, entweder durch Aufrufen der Konfigurationsfunktion mit `-?` oder mithilfe des Cmdlets [Get-Help](https://technet.microsoft.com/en-us/library/hh849696.aspx). Weitere Informationen zur kommentarbasierten Hilfe für PowerShell finden Sie unter [about_Comment_Based_Help](https://technet.microsoft.com/en-us/library/hh847834.aspx).

Das folgende Beispiel zeigt ein Skript, das eine Konfiguration und kommentarbasierte Hilfe dafür enthält:

```powershell
<#
.SYNOPSIS
A brief description of the function or script. This keyword can be used only once for each configuration.


.DESCRIPTION
A detailed description of the function or script. This keyword can be used only once for each configuration.


.PARAMETER ComputerName
The description of a parameter. Add a .PARAMETER keyword for each parameter in the function or script syntax.

Type the parameter name on the same line as the .PARAMETER keyword. Type the parameter description on the lines following the .PARAMETER keyword. 
Windows PowerShell interprets all text between the .PARAMETER line and the next keyword or the end of the comment block as part of the parameter description. 
The description can include paragraph breaks.

The Parameter keywords can appear in any order in the comment block, but the function or script syntax determines the order in which the parameters 
(and their descriptions) appear in help topic. To change the order, change the syntax.

.PARAMETER FilePath
Provide a PARAMETER section for each parameter that your script or function accepts.

.EXAMPLE
A sample command that uses the function or script, optionally followed by sample output and a description. Repeat this keyword for each example. If you have multiple examples,
there is no need to number them. PowerShell will number the examples in help text.

.EXAMPLE
This example will be labeled "EXAMPLE 2" when help is displayed to the user.
#>

configuration HelpSample1
{
    param([string]$ComputerName,[string]$FilePath)
    File f
    {
        Contents="Hello World"
        DestinationPath = "c:\Destination.txt"
    }
}
```

## Anzeigen von Hilfe zur Konfiguration

Verwenden Sie zum Anzeigen der Hilfe für eine Konfiguration das Cmdlet **Get-Help** mit dem Namen der Funktion, oder geben der Namen der Funktion gefolgt von `-?` ein. Folgendes ist die Ausgabe der vorherigen Funktion bei Übergabe an **Get-Help**:

```powershell
PS C:\> Get-Help HelpSample1

NAME
    HelpSample1
    
SYNOPSIS
    A brief description of the function or script. This keyword can be used only once for each configuration.
    
    
SYNTAX
    HelpSample1 [[-InstanceName] <String>] [[-DependsOn] <String[]>] [[-OutputPath] <String>] [[-ConfigurationData] <Hashtable>] [[-ComputerName] 
    <String>] [[-FilePath] <String>] [<CommonParameters>]
    
    
DESCRIPTION
    A detailed description of the function or script. This keyword can be used only once for each configuration.
    

RELATED LINKS

REMARKS
    To see the examples, type: "get-help HelpSample1 -examples".
    For more information, type: "get-help HelpSample1 -detailed".
    For technical information, type: "get-help HelpSample1 -full".
```

## Weitere Informationen
* [DSC-Konfigurationen](configurations.md)




<!--HONumber=Oct16_HO1-->


