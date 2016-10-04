---
title: Trennen von Konfiguration und Umgebungsdaten
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 8a3ae5fdf5d70de999ca6b992efb14533408c305

---

# Trennen von Konfiguration und Umgebungsdaten

>Gilt für: Windows PowerShell 4.0, Windows PowerShell 5.0

In Windows PowerShell DSC ist es möglich, Konfigurationsdaten von der Logik der Konfiguration zu trennen. Eine weitere Betrachtungsweise ist, die strukturelle Konfiguration (z. B. eine Konfiguration, die die Installation von IIS voraussetzt) als von der Umgebungskonfiguration getrennt zu betrachten (also ob die Situation eine Testumgebung oder eine Produktionsumgebung ist. Die Knotennamen wären unterschiedlich, aber die darauf angewendete Konfiguration wäre identisch). Dies bietet die folgenden Vorteile:

* Sie können die Konfigurationsdaten für die verschiedenen Ressourcen, Knoten und Konfigurationen wiederverwenden.
* Konfigurationslogik ist wiederverwendbarer, wenn sie keine hartcodierten Daten enthält. Dies gleicht guten Richtlinien der Skripterstellung für Funktionen.
* Wenn einige der Daten geändert werden müssen, können Sie die Änderungen an einer einzigen Stelle vornehmen und dadurch Zeit sparen und Fehler verringern.

## Grundlegende Konzepte und Beispiele

Um den Umgebungsteil der Konfiguration festzulegen, verwendet DSC den Parameter **ConfigurationData**, der eine Hashtabelle darstellt (bzw. eine PSD1-Datei, die die Hashtabelle enthält). Diese Hashtabelle muss mindestens einen Schlüssel enthalten (**AllNodes**), der über einen strukturierten Wert verfügt. Beispiel:

```powershell
$MyData = 
@{
    AllNodes = @()
    NonNodeData = ""   
}
```

Notieren Sie den AllNodes-Schlüssel, dessen Wert ein Array ist. Jedes Element dieses Arrays ist auch eine Hashtabelle, die „NodeName“ als Schlüssel erfordert:

```powershell
$MyData = 
@{
    AllNodes = 
    @(
        @{
            NodeName = "VM-1"
        },

 
        @{
            NodeName = "VM-2"
        },

 
        @{
            NodeName = "VM-3"
        }
    );

    NonNodeData = ""   
}
```

Jeder Eintrag der Hashtabelle in „AllNodes“ entspricht Konfigurationsdaten für einen Knoten in der Konfiguration. Zusätzlich zu dem erforderlichen NodeName-Schlüssel können Sie der Hashtabelle weitere Schlüssel hinzufügen, z. B.:

```powershell
$MyData = 
@{
    AllNodes = 
    @(
        @{
            NodeName = "VM-1"
            Role     = "WebServer"
        },

 
        @{
            NodeName = "VM-2"
            Role     = "SQLServer"
        },

 
        @{
            NodeName = "VM-3"
            Role     = "WebServer"
        }
    );

    NonNodeData = ""   
}
```

DSC stellt drei spezielle Variablen bereit, die Sie im Konfigurationsskript verwenden können:

* **$AllNodes**: Bezieht sich auf alle Knoten. Mit **.Where()** und **.ForEach()** können Sie Die Auswahl filtern. Anstatt also die Knoten für eine bestimmte Aktion mithilfe von hartcodierten Knotennamen auszuwählen, können Sie beispielsweise folgenden Code schreiben, um im oben stehenden Beispiel VM-1 und VM-3 auszuwählen:

```powershell
configuration MyConfiguration
{
    node $AllNodes.Where{$_.Role -eq "WebServer"}.NodeName
    {
    }
}
```

* **$Node**: Nachdem Sie die Gruppe der Knoten gefiltert haben, können Sie mit „$Node“ auf einen bestimmten Eintrag verweisen. Beispiel:

```powershell
configuration MyConfiguration
{
    Import-DscResource -ModuleName xWebAdministration -Name MSFT_xWebsite
    node $AllNodes.Where{$_.Role -eq "WebServer"}.NodeName
    {
        xWebsite Site
        {
            Name         = $Node.SiteName
            PhysicalPath = $Node.SiteContents
            Ensure       = "Present"
        }
    }
}
```

Um allen Knoten eine Eigenschaft zuzuweisen, Legen Sie „NodeName = *“ fest. Verwenden Sie z. B. folgenden Code, um allen Knoten die Eigenschaft „LogPath“ zuzuweisen:

```
$MyData = 
@{
    AllNodes = 
    @(
        @{
            NodeName           = "*"
            LogPath            = "C:\Logs"
        },

 
        @{
            NodeName = "VM-1"
            Role     = "WebServer"
            SiteContents = "C:\Site1"
            SiteName = "Website1"
        },

 
        @{
            NodeName = "VM-2"
            Role     = "SQLServer"
        },

 
        @{
            NodeName = "VM-3"
            Role     = "WebServer"
            SiteContents = "C:\Site2"
            SiteName = "Website3"
        }
    );
}
```

* **$ConfigurationData**: Sie können diese Variable in einer Konfiguration verwenden, um auf die als Parameter übergebene Konfigurationsdaten-Hashtabelle zuzugreifen. Beispiel:

```powershell
$MyData = 
@{
    AllNodes = 
    @(
        @{
            NodeName           = "*"
            LogPath            = "C:\Logs"
        },

 
        @{
            NodeName = "VM-1"
            Role     = "WebServer"
            SiteContents = "C:\Site1"
            SiteName = "Website1"
        },

 
        @{
            NodeName = "VM-2"
            Role     = "SQLServer"
        },
 

        @{
            NodeName = "VM-3"
            Role     = "WebServer"
            SiteContents = "C:\Site2"
            SiteName = "Website3"
        }
    );

    NonNodeData = 
    @{
        ConfigFileContents = (Get-Content C:\Template\Config.xml)
     }   
} 

configuration MyConfiguration
{
    Import-DscResource -ModuleName xWebAdministration -Name MSFT_xWebsite

    node $AllNodes.Where{$_.Role -eq "WebServer"}.NodeName
    {
        xWebsite Site
        {
            Name         = $Node.SiteName
            PhysicalPath = $Node.SiteContents
            Ensure       = "Present"
        }

        File ConfigFile
        {
            DestinationPath = $Node.SiteContents + "\\config.xml"
            Contents = $ConfigurationData.NonNodeData.ConfigFileContents
        }
    }
}
```

Ein vollständiges Beispiel finden Sie im [xWebAdministration-Modul](https://powershellgallery.com/packages/xWebAdministration).




<!--HONumber=Oct16_HO1-->


