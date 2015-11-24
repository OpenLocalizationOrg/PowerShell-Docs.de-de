#Das Einrichten eines DSC Pull-Webservers

> Gilt für: WindowsPowerShell 4.0, WindowsPowerShell 5.0

Ein DSC Pull-Webserver ist ein Webdienst in IIS, die eine OData-Schnittstelle verwendet, um Konfigurationsdateien DSC Zielknoten zur Verfügung, wenn die Knoten für diese Fragen.

Anforderungen für die Verwendung von eines Pull-Servers:

* Ein Server mit:
   - WMF/PowerShell 4.0 oder höher
   - IIS-Serverrolle
   - DSC-Dienst
* Im Idealfall einige bedeutet, dass der Generierung eines Zertifikats zum Sichern der Anmeldeinformationen auf die lokale Configuration Manager (LCM) Zielknoten

Sie können die IIS-Serverrolle und DSC-Dienst mit dem Assistenten für Hinzufügen von Rollen und Features im Server-Manager oder mithilfe von PowerShell hinzufügen. Die Beispielskripts, die in diesem Thema enthalten behandelt beide Schritte für Sie ebenfalls.

##Verwenden die xWebService-Ressource

Die einfachste Möglichkeit zum Einrichten eines Webservers für Pullabonnements ist die Verwendung von xWebService-Ressource, die im Modul "xPSDesiredStateConfiguration" enthalten. Die folgenden Schritte wird erläutert, wie die Ressource in einer Konfiguration verwenden, die den Webdienst einrichtet.

1. Rufen Sie die [Install-Module](https://technet.microsoft.com/en-us/library/dn807162.aspx) -Cmdlet zum Installieren der **xPSDesiredStateConfiguration** Modul. **Hinweis**: **Install-Module** befindet sich auf der **PowerShellGet** Module, die in PowerShell 5.0 enthalten ist. Download der **PowerShellGet** -Modul für PowerShell 3.0 und 4.0 auf [PackageManagement PowerShell-Module Vorschau](https://www.microsoft.com/en-us/download/details.aspx?id=49186).
1. Erstellen Sie ein selbstsigniertes Zertifikat mit dem Betreff `"CN = PSDSCPullServerCert"` in den `CERT: \LocalMachine\MY\` zu speichern. Sie erreichen dies mit dem Befehl `'CERT: \LocalMachine\MY' New-SelfSignedCertificate - CertStoreLocation-Betreff "CN = PSDSCPullServerCert'`.
1. In der PowerShell ISE starten (F5) das folgende Skript (in den Dateien entpackt als Sample_xDscWebService.ps1 enthalten). Dieses Skript richtet die Pull-Servers und eines Compliance-Servers ein.

```powershell
Configuration Sample_xDSCService
{
    param (
    [ValidateNotNullOrEmpty()]
    [String] $certificateThumbPrint
    )
    Import-DSCResource –ModuleName DSCServic
    Node localhost
    {
        WindowsFeature DSCServiceFeature
        {
            Ensure = “Present”
            Name = “DSC-Service”
        }

        DSCService PSDSCPullServer
        {
            Ensure   = "Present"
            Name     = “PSDSCPullServer”
            Port     = 8080
            PhysicalPath = "$env:SystemDrive\inetpub\wwwroot\PSDSCPullServer"
            EnableFirewallException = $true
            CertificateThumbprint = $certificateThumbPrint
            ModulePath = “$env:PROGRAMFILES\WindowsPowerShell\DscService\Modules”
            ConfigurationPath = “$env:PROGRAMFILES\WindowsPowerShell\DscService\Configuration”
            State = “Started”
            DependsOn = “[WindowsFeature]DSCServiceFeature             
        }
    }
}
```

1. Führen Sie die Konfiguration, übergeben den Fingerabdruck des selbstsignierte-Zertifikat Sie erstellt haben, als das **CertificateThumbPrint** Parameter:

```powershell
PS:\>$myCert = Get-ChildItem CERT: | Where {$_.Subject -eq 'CN=PSDSCPullServerCert'}
PS:\>Sample_xDSCService -certificateThumbprint $myCert.Thumbprint 
```

##Registrierungsschlüssel

Damit Clientcomputer Knoten, die mit dem Server zu registrieren, sodass sie Konfigurationsnamen anstelle einer Konfigurations-ID, einen Registrierungsschlüssel verwenden können (die eine GUID ist bekannt ist, dass der Server und der Clientknoten) platziert werden muss, in einer Datei namens `RegistrationKeys.txt`. Standardmäßig erwartet der Pull-Server, die durch dieses Beispiel erstellte Datei befinden, in `c:\Programme\Microsoft Files\WindowsPowerShell\DscService`. Erstellen Sie eine Textdatei mit jeweils einer Zeile, die den Registrierungsschlüssel aus, und speichern Sie sie in diesem Ordner.
> **Hinweis**: Registrierungsschlüssel werden in PowerShell 4.0 nicht unterstützt.

##Platzieren von Konfigurationen und Ressourcen

Nach Abschluss der Installation die Pull-Server ist es ein neuer Ordner unter `$env: PROGRAMFILES\WindowsPowerShell` mit dem Namen "DscService". Es gibt zwei Ordner mit dem Namen "Module" und "Konfiguration", in diesem Ordner. Platzieren Sie alle Ressourcen, die für Konfigurationen erforderlich sind, die von diesem Server per-Knoten Pull, in den Ordner "Module". Platzieren Sie die MOF-Konfigurationsdateien für alle Konfigurationen, die vom Knoten abgerufen werden sollen, im Ordner "Configuration". Die Namen der MOF-Dateien hängt vom Typ des Pull-Clients. Die folgenden Themen beschreiben das Einrichten von Pull-Clients im Detail:

* [Einrichten eines DSC Pull-Clients mithilfe des Konfigurations-ID](pullClientConfigID.md)
* [Einrichten eines mit Konfigurationsnamen DSC Pull-Clients](pullClientConfigNames.md)
* [Teilweise Konfigurationen](partialConfigs.md)

##Siehe auch

* [WindowsPowerShell gewünschten Status (Übersicht)](overview.md)
* [Einführung von Konfigurationen](enactingConfigurations.md)
* [Gewusst wie: Knoteninformationen vom DSC Pull-Server abgerufen werden.](retrieveNodeInfo.md)



