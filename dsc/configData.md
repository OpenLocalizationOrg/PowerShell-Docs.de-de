#Trennen von Konfiguration und Umgebungsdaten

> Gilt für: WindowsPowerShell 4.0, WindowsPowerShell 5.0

In Windows PowerShell gewünscht State Configuration (DSC), ist es möglich, die von der Logik der Konfiguration der Konfigurationsdaten zu trennen. Eine andere Möglichkeit dafür ist die strukturelle Konfiguration berücksichtigen (z. B. eine Konfiguration möglicherweise, dass IIS installiert werden) als separate aus der Umwelt-Konfiguration (gibt an, ob die Situation einer Umgebung im Vergleich zu einer Produktion ist – wären die Knotennamen, aber die Konfiguration angewendet wird, wären die gleichen). Dies hat folgende Vorteile:

* Sie können die Konfigurationsdaten für verschiedene Ressourcen, Knoten und Konfigurationen wiederverwendet.
* Konfiguration ist einfacher wiederverwenden, wenn sie keine hartcodierte Daten enthält. Dies ist ähnlich sinnvolle scripting Richtlinien für Funktionen.
* Wenn einige der Daten ändern muss, können Sie die Änderungen an einem Ort vornehmen, dadurch sparen Sie Zeit und Verringern von Fehlern.

##Grundlegende Konzepte und Beispiele

An die Umwelt Teil der Konfiguration, DSC verwendet die **ConfigurationData** -Parameter ist eine Hash-Tabelle (oder eine psd1-Datei enthält die Hashtabelle akzeptiert). Dieser Hashtabelle muss mindestens ein Schlüssel haben **AllNodes**, die über einen strukturierten Wert verfügt. Beispiel:

```powershell
$MyData = 
@{
    AllNodes = @();
    NonNodeData = ""   
}
```

Beachten Sie den Schlüssel AllNodes, deren Wert ein Array ist. Jedes Element dieses Arrays ist auch eine Hash-Tabelle, die Knotennamen als Schlüssel erforderlich ist:

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

Jeder Eintrag des Hash-Tabelle in AllNodes entspricht Konfigurationsdaten zu einem Knoten in der Konfiguration. Zusätzlich zu den erforderlichen NodeName-Schlüssel können Sie andere Schlüssel der Hashtabelle, z. B. hinzufügen:

```powershell
$MyData = 
@{
    AllNodes = 
    @(
        @{
            NodeName = "VM-1";

        },


        @{
            NodeName = "VM-2";
            Role     = "SQLServer"
        },


        @{
            NodeName = "VM-3";
            Role     = "WebServer"
        }
    );

    NonNodeData = ""   
}
```

DSC bietet drei spezielle Variablen in das Skript verwenden:

* **$AllNodes**: bezieht sich auf alle Knoten. Filtern mit können **. WHERE()** und **. ForEach()**, sodass hart codierte Knotennamen bestimmte Knoten für die Aktion auswählen, Sie etwa Folgendes VM-1 und VM-3 Wählen Sie im obigen Beispiel schreiben können:

```powershell
configuration MyConfiguration
{
    node $AllNodes.Where{$_.Role -eq "WebServer"}.NodeName
    {
    }
}
```

* **$Node**: Sobald Knoten gefiltert wird, können Sie $Node um auf den entsprechenden Eintrag zu verweisen. Beispiel:

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

Um alle Knoten eine Eigenschaft zuzuweisen, können Sie festlegen, NodeName = "*". Beispielsweise könnte jeder Knoten die LogPath-Eigenschaft erteilen möchten, Sie dies durchführen:

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
            NodeName = "VM-1";
            Role     = "WebServer"
            SiteContents = "C:\Site1"
            SiteName = "Website1"
        },


        @{
            NodeName = "VM-2";
            Role     = "SQLServer"
        },


        @{
            NodeName = "VM-3";
            Role     = "WebServer";
            SiteContents = "C:\Site2"
            SiteName = "Website3"
        }
    );
}
```

* **$ConfigurationData**: Sie können diese Variable in einer Konfiguration Zugriff auf die Konfiguration Daten Hash-Tabelle als Parameter übergeben. Beispiel:

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
            NodeName = "VM-1";
            Role     = "WebServer"
            SiteContents = "C:\Site1"
            SiteName = "Website1"
        },


        @{
            NodeName = "VM-2";
            Role     = "SQLServer"
        },


        @{
            NodeName = "VM-3";
            Role     = "WebServer";
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

Finden Sie ein vollständiges Beispiel enthalten, die der [xWebAdministration Modul](https://powershellgallery.com/packages/xWebAdministration).



