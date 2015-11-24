#Beginnen Sie mit dem gewünschten State Configuration (DSC) für Linux

In diesem Thema wird erläutert, wie die ersten Schritte mit PowerShell gewünscht State Configuration (DSC) für Linux. Allgemeine Informationen zu DSC, finden Sie unter [Erste Schritte mit Windows PowerShell gewünschten Konfiguration](overview.md).

##Unterstützte Linux-Vorgang System-Versionen

Die folgenden Versionen des Linux-Betriebssystems werden für DSC für Linux unterstützt.
- CentOS 5, 6 und 7 (X 86/X 64)
- Debian GNU/Linux 6 und 7 (X 86/X 64)
- Oracle Linux 5, 6 und 7 (X 86/X 64)
- Red Hat Enterprise Linux Server 5, 6 und 7 (X 86/X 64)
- SUSE Linux Enterprise Server 10, 11 und 12 (X 86/X 64)
- Ubuntu Server 12.04 LTS und 14.04 LTS (X 86/X 64)

Die folgende Tabelle beschreibt die erforderlichen Package Abhängigkeiten für DSC für Linux.

| Erforderliches Paket| Beschreibung| Mindestversion|
|---|---|---|
| glibc| GNU-Bibliothek| 2…4 – 31.30|
| Python| Python| 2.4 – 3.4|
| "omiserver"| Open-Infrastruktur| 1.0.8.1|
| OpenSSL| OpenSSL-Bibliotheken| 0.9.8 oder 1.0|
| ctypes| Python-CTypes-Bibliothek| Python-Version übereinstimmen|
| libcurl| http-Clientbibliothek cURL| 7.15.1|

##Installieren von DSC für Linux

Installieren Sie die [Open Management Infrastructure (OMI)](https://collaboration.opengroup.org/omi/) vor der Installation von DSC für Linux.

###Installieren von OMI

Konfiguration des gewünschten für Linux ist den Open Management Infrastructure (OMI) CIM-Server Version 1.0.8.1 erforderlich. Von The Open Group OMI heruntergeladen werden kann: [Open Management Infrastructure (OMI)](https://collaboration.opengroup.org/omi/).

Um OMI zu installieren, installieren Sie das Paket, das für Ihre Linux-Systemen (.rpm oder .deb) und OpenSSL-Version geeignet ist (Ssl_098 oder Ssl_100), und die Architektur (X 64/X 86). RPM-Pakete sind für CentOS, Red Hat Enterprise Linux, SUSE Linux Enterprise Server und Oracle Linux geeignet. DEB-Pakete sind für Debian GNU/Linux und Ubuntu Server geeignet. Ssl_098 Pakete eignen sich für Computer mit OpenSSL Ssl installiert 0.9.8_100 Pakete eignen sich für Computer mit OpenSSL 1.0 installiert.

> **Hinweis**: Führen Sie den Befehl zum Bestimmen der installierten Version des OpenSSL `Openssl-Version`.

Führen Sie den folgenden Befehl OMI auf einer CentOS 7 X 64-System zu installieren.

`# Sudo Rpm - Uvh "omiserver"-1.0.8.ssl_100.rpm`

###Installieren von DSC

Um DSC zu installieren, installieren Sie das Paket, das für Ihre Linux-Systemen (.rpm oder .deb) und OpenSSL-Version geeignet ist (Ssl_098 oder Ssl_100), und die Architektur (X 64/X 86). RPM-Pakete sind für CentOS, Red Hat Enterprise Linux, SUSE Linux Enterprise Server und Oracle Linux geeignet. DEB-Pakete sind für Debian GNU/Linux und Ubuntu Server geeignet. Ssl_098 Pakete eignen sich für Computer mit OpenSSL Ssl installiert 0.9.8_100 Pakete eignen sich für Computer mit OpenSSL 1.0 installiert.

> **Hinweis**: Führen Sie zum Bestimmen der installierten Version des OpenSSL die Befehl Openssl-Version.

Führen Sie den folgenden Befehl DSC auf einer CentOS 7 X 64-System zu installieren.

`# Sudo dsc Rpm - Uvh für-1.0.0-254.ssl_100.x64.rpm`


##Verwenden von DSC für Linux

In den folgenden Abschnitten wird das Erstellen und Ausführen von DSC-Konfigurationen auf Linux-Computern erläutert.

###Erstellen ein Dokument zur MOF-Konfiguration

Das Konfiguration der Windows PowerShell-Schlüsselwort wird verwendet, um eine Konfiguration für Linux-Computer genauso wie für Windows-Computer zu erstellen. Die folgenden Schritte beschreiben das Erstellen eines Dokuments Konfiguration für eine Linux-Computer mithilfe von Windows PowerShell.

1. Importieren Sie das Nx-Modul. Nx Windows PowerShell-Modul muss enthält das Schema für integrierte Ressourcen für DSC für Linux und auf dem lokalen Computer installiert und in der Konfiguration importiert.
   
   -Kopieren Sie das Modulverzeichnis Nx um die Nx-Modul installieren, entweder `%UserProfile%\Documents\WindowsPowerShell\Modules\` oder `C:\windows\system32\WindowsPowerShell\v1.0\Modules`. Nx-Modul ist in der DSC für Linux-Installationspaket (MSI) enthalten. Verwenden Sie zum Importieren des Nx-Moduls in der Konfiguration der __Import DSCResource__ Befehl:

```powershell
Configuration ExampleConfiguration{

    Import-DSCResource -Module nx

}
```
2. Definieren Sie eine Konfiguration und generieren Sie Konfigurationsdokument:

```powershell
Configuration ExampleConfiguration{

    Import-DSCResource -Module nx

    Node  "linuxhost.contoso.com"{
    nxFile ExampleFile {

        DestinationPath = "/tmp/example"
        Contents = "hello world `n"
        Ensure = "Present"
        Type = "File"
    }

    }
}
ExampleConfiguration -OutputPath:"C:\temp" 
```

###Drücken Sie die Konfiguration auf dem Linux-computer

Konfigurationsdokumente (MOF-Dateien) abgelegt werden können, auf dem Linux-Computer unter Verwendung der [Start DscConfiguration](https://technet.microsoft.com/en-us/library/dn521623.aspx) Cmdlet. Um dieses Cmdlet zu verwenden, zusammen mit den [Get-DscConfiguration](https://technet.microsoft.com/en-us/library/dn407379).aspx, oder [Test DscConfiguration](https://technet.microsoft.com/en-us/library/dn407382.aspx) -Cmdlets Remote auf einem Linux-Computer müssen, verwenden Sie eine CIMSession. Die [New-CimSession](https://technet.microsoft.com/en-us/library/jj590760.aspx) Cmdlet wird verwendet, um eine CIMSession auf dem Linux-Computer zu erstellen.

Der folgende Code zeigt, wie eine CIMSession für DSC für Linux zu erstellen.

```powershell
$Node = "ostc-dsc-01"
$Credential = Get-Credential -UserName:"root" -Message:"Enter Password:"

#Ignore SSL certificate validation
#$opt = New-CimSessionOption -UseSsl:$true -SkipCACheck:$true -SkipCNCheck:$true -SkipRevocationCheck:$true

#Options for a trusted SSL certificate
$opt = New-CimSessionOption -UseSsl:$true 
$Sess=New-CimSession -Credential:$credential -ComputerName:$Node -Port:5986 -Authentication:basic -SessionOption:$opt -OperationTimeoutSec:90 
```

> **Note**:
* Für den Modus "Push" muss die Benutzeranmeldeinformationen der Root-Benutzer auf dem Linux-Computer sein.
* Für DSC werden nur SSL/TLS-Verbindungen unterstützt, Linux, das New-CimSession muss mit dem auf $true festgelegt – UseSSL-Parameter verwendet werden.
* Das SSL-Zertifikat, das zum durch OMI (DSC) ist in der Datei angegeben: `/opt/omi/etc/omiserver.conf` mit den Eigenschaften: Pemfile und Keyfile.
   Wenn dieses Zertifikat nicht vom Windows-Computer als vertrauenswürdig eingestuft wird, die Sie verwenden die [New-CimSession](https://technet.microsoft.com/en-us/library/jj590760.aspx) Cmdlet auf, können Sie die Überprüfung des Zertifikats mit den Optionen CIMSession ignorieren: `- SkipCACheck: $true - SkipCNCheck: $true - SkipRevocationCheck: $true`

Führen Sie den folgenden Befehl aus, um die DSC-Konfiguration auf den Linux-Knoten zu übertragen.

`Start DSCConfiguration-Pfad: "C:\temp" - Cimsession: $sess-wait - verbose`

###Verteilen Sie die Konfiguration mit einem Pull-server

Konfigurationen können auf einem Linux-Computer mit einem Pull-Server, genau wie für Windows-Computer verteilt werden. Anleitung zur Verwendung von eines Pull-Servers, finden Sie unter [Windows PowerShell gewünschten Status Pull Konfigurationsserver](pullServer.md). Weitere Informationen und Einschränkungen hinsichtlich der Verwendung von Linux-Computer mit einem Pull-Server finden Sie die Versionshinweise für die gewünschte Konfiguration für Linux.

###Arbeiten mit Konfigurationen lokal

DSC für Linux umfasst Skripts zur Konfiguration aus dem lokalen Linux-Computer arbeiten. Diese Skripts sind, suchen Sie in `/opt/microsoft/dsc/Scripts` umfassen Folgendes:
* GetConfiguration.py
   
   Gibt die aktuelle Konfiguration, die auf den Computer angewendet. Ähnlich wie die Windows PowerShell-Cmdlet "Get-DscConfiguration"-Cmdlet.

`# Sudo./GetConfiguration.py`

* GetMetaConfiguration.py
   
   Gibt die aktuelle Meta-Konfiguration auf dem Computer angewendet. Das Cmdlet ähnelt [Get-DSCLocalConfigurationManager](https://technet.microsoft.com/en-us/library/dn407378.aspx) Cmdlet.

`# Sudo./GetMetaConfiguration.py`

* InstallModule.py
   
   Wird ein benutzerdefiniertes DSC Ressource-Modul installiert. Erfordert den Pfad zur ZIP-Datei der Modul shared Objects-Bibliothek und MOF-Dateien.

`# Sudo./InstallModule.py /tmp/cnx_Resource.zip`

* RemoveModule.py
   
   Entfernt ein benutzerdefiniertes DSC Ressource-Modul. Erfordert den Namen des Moduls zu entfernen.

`# Sudo./RemoveModule.py cnx_Resource`

* SendConfigurationApply.py
   
   Eine MOF-Konfigurationsdatei auf den Computer angewendet. Ähnlich wie die [Start DscConfiguration](https://technet.microsoft.com/en-us/library/dn521623.aspx) Cmdlet. Erfordert den Pfad der Konfiguration MOF anwenden.

`# Sudo./RemoveModule.py cnx_Resource`

* SendMetaConfiguration.py
   
   Eine Meta-Konfiguration-MOF-Datei auf den Computer angewendet. Ähnlich wie die [Set DSCLocalConfigurationManager](https://technet.microsoft.com/en-us/library/dn521621.aspx) Cmdlet. Erfordert den Pfad der MOF-Meta-Konfiguration anwenden.

`# Sudo./SendMetaConfiguration.py Configurationmof – /tmp/localhost.meta.mof`

##PowerShell Konfigurieren des gewünschten Zustands für Linux-Protokolldateien

Die folgenden Protokolldateien werden für Linux-Nachrichten für DSC generiert.

| Protokolldatei| Directory| Beschreibung|
|---|---|---|
| omiserver.log| / opt/Omi/Var/Log /| Nachrichten, die im Zusammenhang mit dem Betrieb des Servers OMI CIM.|
| DSC.log| / opt/Omi/Var/Log /| Nachrichten, die im Zusammenhang mit den Vorgang der Ressource lokale Configuration Manager (LCM) und DSC.|




