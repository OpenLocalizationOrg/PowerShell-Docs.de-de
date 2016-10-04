---
title: "Überprüfen der Signatur von DSC-Modulen und -Konfigurationen"
author: jaimeo
contributor: berheabra
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 817fadb79716e41ce8cc8f4245dedc66347ac413

---

##Überprüfen der Signatur von DSC-Modulen und -Konfigurationen
In DSC werden Dokumente und Module von Pull-Server oder pullserver (im Fall von Azure Automation-Pull-Dienst) auf verwalteten Computer verteilt. Die Gefährdung der Pull-Server/Dienst kann ein Angreifer potenziell Ändern der Konfigurationen und auf dem pullserver Module und es an alle verwalteten Computer verteilt beeinträchtigen daher sogar noch mehr Computer des Kunden. 

 Diese Bedrohung wird im WMF 5.1 beschrieben. DSC unterstützt Überprüfen der digitalen Signaturen von Modulen und Konfiguration (. MOF-Dateien). Diese Funktion wird verhindert, dass Knoten ausgeführt wird, eine Konfiguration oder Moduldateien, die nicht von vertrauenswürdigen Signaturgeber signiert werden oder, manipuliert wurde, nachdem diese von vertrauenswürdigen Signaturgeber signiert wurden. 



###So signieren Sie Konfigurationen und Module 
***
1. Konfigurationsdateien (MOF-Dateien): Das vorhandene PowerShell-Cmdlet [Set-AuthenticodeSignature](https://technet.microsoft.com/library/hh849819.aspx) wurde erweitert und unterstützt nun das Signieren von MOF-Dateien.  
2. Module:- Module werden signiert, indem der entsprechende Modulkatalog wie in den nachfolgenden Schritten erläutert signiert wird:- 
    * Erstellen einer Katalogdatei: Eine Katalogdatei enthält eine Sammlung kryptografischer Hashes oder Fingerabdrücke. Jeder Fingerabdruck entspricht einer Datei, die im Modul enthalten ist.  Mit dem neu hinzugefügten Cmdlet [New-FileCatalog](https://technet.microsoft.com/library/cc732148.aspx) können Benutzer eine Katalogdatei für ihr Modul erstellen. Die Katalog-Cmdlets finden Sie unter [eine relative Verknüpfung](catalog-cmdlets.md) Katalogdateien zu erstellen. 
    * Signieren der Katalogdatei: Mit [Set-AuthenticodeSignature](https://technet.microsoft.com/library/hh849819.aspx) wird die Katalogdatei signiert.
    * Fügen Sie die Katalogdatei in den Modulordner ein.
Gemäß der Konvention sollte die Modulkatalogdatei mit dem gleichen Namen wie das Modul im Modulordner abgelegt werden.

###Einstellungen im lokalen Konfigurations-Manager (Local Configuration Manager; LCM) zur Aktivierung von Signaturüberprüfungen

####PULL
Der lokale Konfigurations-Manager eines Knotens überprüft die Signatur von Modulen und Konfigurationen auf Grundlage der aktuellen Einstellungen. Die Überprüfung der Signatur ist standardmäßig deaktiviert. Die Signaturüberprüfung kann aktiviert werden, indem Sie wie unten dargestellt den Block „SignatureValidation“ der Definition der Metakonfiguration des Knotens hinzufügen:-

```PowerShell
[DSCLocalConfigurationManager()]
Configuration EnableSignatureValidation
{
    Settings
    {
        RefreshMode = 'PULL'        
    } 
    
    ConfigurationRepositoryWeb pullserver{
      ConfigurationNames = 'sql'
      ServerURL = 'http://localhost:8080/PSDSCPullServer/PSDSCPullServer.svc'
      AllowUnsecureConnection = $true
      RegistrationKey = 'd6750ff1-d8dd-49f7-8caf-7471ea9793fc' # Replace this with correct registration key.
    }
    SignatureValidation validations{
        # By default,LCM will use default Windows trusted publisher store to validate the certificate chain. If TrustedStorePath property is specified, LCM will use this custom store for retrieving the trusted publishers to validate the content.
        TrustedStorePath = 'Cert:\LocalMachine\DSCStore'            
        SignedItemType =  'Configuration','Module'         # Those are list of DSC artifacts, for which LCM need to verify their digital signature before executing them on the node.       
    }
 
}
EnableSignatureValidation
Set-DscLocalConfigurationManager -Path .\EnableSignatureValidation -Verbose 
 ```

Die Festlegung der oben genannten Metakonfiguration auf einem Knoten aktiviert die Überprüfung der Signatur für heruntergeladene Konfigurationen und Module. Der lokale Konfigurations-Manager führt die folgenden Schritte aus, um die digitalen Signaturen zu überprüfen.
* Überprüfen, ob die Signatur einer Konfigurationsdatei (MOF) gültig ist. Der lokale Konfigurations-Manager verwendet das PowerShell-Cmdlet [Get-AuthenticodeSignature](https://technet.microsoft.com/library/hh849805.aspx), das in 5.1 zur Unterstützung der Überprüfung von MOF-Signaturen erweitert wurde.
* Überprüfen der Zertifizierungsstelle, die den Signaturgeber als vertrauenswürdig eingestuft hat.
* Herunterladen der Modul-/Ressourcenabhängigkeiten der Konfiguration in einen temporären Speicherort.
* Überprüfen der Signatur des Katalogs, der im Modul enthalten ist
    * Suchen einer „<moduleName>.cat“-Datei und Überprüfen der Signatur mithilfe des Cmdlets [Get-AuthenticodeSignature](https://technet.microsoft.com/library/hh849805.aspx)
    * Überprüfen der Zertifizierungsstelle, die den Signaturgeber als vertrauenswürdig eingestuft hat
    * Sicherstellen, dass der Inhalt der Module nicht mit dem neuen Cmdlet [Test-FileCatalog](https://technet.microsoft.com/library/cc732148.aspx) manipuliert wurde.
* Installieren des Moduls mithilfe des Cmdlets „Install-Module“ im Verzeichnis „$env:ProgramFiles\WindowsPowerShell\Modules\“
* Prozesskonfiguration

Hinweis: - Signaturen von Modulkatalogen und -konfigurationen werden nur bei der ersten Anwendung der Konfiguration auf das System überprüft, oder wenn das Modul heruntergeladen und installiert wird. Der Konsistenzlauf überprüft nicht die Signatur von „current.mof“ oder der Modulabhängigkeiten.
Wenn die Überprüfung in jeder Phase fehlgeschlagen ist, für die Instanz, wenn die Konfiguration aus dem Pullserver abgerufen ohne Vorzeichen, und dann die Verarbeitung der Konfiguration mit den nachfolgend aufgeführten Fehler beendet, und alle temporäre Dateien werden gelöscht.

![Beispielkonfiguration einer Fehlerausgabe](../../images/PullUnsignedConfigFail.PNG)

Entsprechend wird beim Übertragen eines Moduls mithilfe von Pull, dessen Katalog nicht signiert ist, der folgende Fehler ausgegeben:-

![Beispielmodul einer Fehlerausgabe](../../images/PullUnisgnedCatalog.PNG)

####PUSH
Eine Konfiguration per Push übermittelt möglicherweise an ihrer Quelle manipuliert werden, bevor sie auf den Knoten bereitgestellt wird. Der lokale Konfigurations-Manager wird für mithilfe von Push übertragene oder veröffentlichte Konfigurationen ähnliche Schritte zur Überprüfung der Signatur durchführen.
Nachstehend finden Sie ein vollständiges Beispiel für die Signaturüberprüfung bei einer mithilfe von Push übertragenen Konfiguration.

* Aktivieren Sie die Überprüfung der Signatur auf dem Knoten.

```Powershell
[DSCLocalConfigurationManager()]
Configuration EnableSignatureValidation
{
    Settings
    {
        RefreshMode = 'PUSH'        
    } 
    SignatureValidation validations{
        TrustedStorePath = 'Cert:\LocalMachine\DSCStore'   
        SignedItemType =  'Configuration','Module'             
    }

}
EnableSignatureValidation
Set-DscLocalConfigurationManager -Path .\EnableSignatureValidation -Verbose
``` 
* Erstellen Sie eine Beispielkonfigurationsdatei.

```Powershell
# Sample configuration
Configuration Test{

    File foo
    {
        DestinationPath =  "$env:TEMP\signingTest.txt"
        Contents = "ABC"
    }
}
Test
```

* Versuchen Sie, die nicht signierte Konfigurationsdatei mithilfe von Push auf den Knoten zu übertragen. 

```Powershell
Start-DscConfiguration -Path .\Test -Wait -Verbose -Force
``` 
![ErrorUnsignedMofPushed](../../images/PushUnsignedMof.PNG)

* Signieren Sie die Konfigurationsdatei mithilfe eines Codesignaturzertifikats.

![SignMofFile](../../images/SignMofFile.PNG)

* Versuchen Sie, die signierte MOF-Datei mithilfe von Push zu übertragen.

![SignMofFile](../../images/PushSignedMof.PNG)




<!--HONumber=Oct16_HO1-->


