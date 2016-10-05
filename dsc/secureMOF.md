---
title: "Schützen der MOF-Datei"
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: f0aed5bb627825b74fe4df29cbe2f0bc53f90c23

---

# Schützen der MOF-Datei

>Gilt für: Windows PowerShell 4.0, Windows PowerShell 5.0

DSC weist die Zielknoten an, welche Konfiguration sie aufweisen sollen, indem eine MOF-Datei mit den gewünschten Informationen an alle Knoten gesendet wird, auf denen der lokale Konfigurations-Manager (LCM) die gewünschte Konfiguration implementiert. Da diese Datei die Details der Konfiguration enthält, muss sie geschützt werden. Zu diesem Zweck können Sie den LCM die Anmeldeinformationen eines Benutzers überprüfen lassen. In diesem Thema wird beschrieben, wie diese Anmeldeinformationen sicher an den Zielknoten übertragen werden, indem sie mithilfe von Zertifikaten verschlüsselt werden.

>**Hinweis:** In diesem Thema werden für die Verschlüsselung verwendete Zertifikate behandelt. Für die Verschlüsselung ist ein selbst signiertes Zertifikat ausreichend, da der private Schlüssel immer geheim ist und die Verschlüsselung die Vertrauenswürdigkeit des Dokuments nicht impliziert. Selbstsignierte Zertifikate sollten *nicht* zu Authentifizierungszwecken verwendet werden. Zum Zweck der Authentifizierung Sie sollten Sie ein Zertifikat von einer vertrauenswürdigen Zertifizierungsstelle verwenden.

## Voraussetzungen

Stellen Sie sicher, dass Folgendes zutrifft, um die Anmeldeinformationen sicher zu verschlüsseln, die zum Schutz einer DSC-Konfiguration dienen:

* **Möglichkeiten zum Ausstellen und Verteilen von Zertifikaten**. In diesem Thema und seinen Beispielen wird davon ausgegangen, dass Sie eine Active Directory-Zertifizierungsstelle verwenden. Weitere Informationen zu Active Directory-Zertifikatdiensten finden Sie unter [Übersicht über Active Directory-Zertifikatdienste](https://technet.microsoft.com/library/hh831740.aspx) und [Active Directory-Zertifikatdienste in Windows Server 2008](https://technet.microsoft.com/windowsserver/dd448615.aspx).
* **Administratorzugriff auf den Zielknoten oder Knoten**.
* **Jeder Zielknoten hat ein verschlüsselungsfähiges Zertifikat, das in seinem persönlichen Speicher gespeichert ist**. In Windows PowerShell ist der Pfad zum Speicher „Cert: \LocalMachine\My“. In den Beispielen in diesem Thema verwenden Sie die Vorlage „Arbeitsstationsauthentifizierung“, die Sie (zusammen mit anderen Zertifikatvorlagen) unter [Standardzertifikatvorlagen](https://technet.microsoft.com/library/cc740061(v=WS.10).aspx) finden.
* Wenn Sie diese Konfiguration auf einem anderen Computer als dem Zielknoten ausführen, **exportieren Sie den öffentlichen Schlüssel des Zertifikats**, und importieren Sie ihn anschließend auf den Computer, auf dem Sie die Konfiguration ausführen. Stellen Sie sicher, dass Sie nur den **öffentlichen** Schlüssel exportieren. Halten Sie den privaten Schlüssel geschützt.

## Allgemeiner Prozess

 1. Richten Sie die Zertifikate, Schlüssel und Fingerabdrücke ein, und stellen Sie sicher, dass jeder Zielknoten über Kopien des Zertifikats verfügt, und dass sich der öffentliche Schlüssel und Fingerabdruck auf dem Konfigurationscomputer befinden.
 2. Erstellen Sie einen „Configuration“-Datenblock, der den Pfad und Fingerabdruck des öffentlichen Schlüssels enthält.
 3. Erstellen Sie ein Konfigurationsskript, das die gewünschte Konfiguration für den Zielknoten definiert. Richten Sie die Entschlüsselung auf den Zielknoten ein, indem Sie den lokalen Konfigurations-Manager anweisen, die Konfigurationsdaten mithilfe des Zertifikats und Fingerabdrucks zu entschlüsseln.
 4. Führen Sie die Konfiguration aus, woraufhin die Einstellungen des lokalen Konfigurations-Managers festgelegt werden und die DSC-Konfiguration gestartet wird.

![Diagramm1](images/CredentialEncryptionDiagram1.png)

## Zertifikatanforderungen

Zum Aktivieren der Verschlüsselung der Anmeldeinformationen muss auf dem _Zielknoten_ ein Zertifikat für öffentliche Schlüssel verfügbar sein, dem der zum Erstellen der DSC-Konfiguration verwendete Computer **vertraut**.
Dieses Zertifikat für öffentliche Schlüssel muss bestimmte Anforderungen erfüllen, damit es für die Verschlüsselung der DSC-Anmeldeinformationen verwendet werden kann:
 1. **Schlüsselverwendung**:
   - Muss enthalten: „KeyEncipherment“ und „DataEncipherment“.
   - Sollte _nicht_ enthalten: „Digitale Signatur“.
 2. **Erweiterte Schlüsselverwendung**:
   - Muss enthalten: Dokumentenverschlüsselung (1.3.6.1.4.1.311.80.1).
   - Sollte _nicht_ enthalten: Clientauthentifizierung (1.3.6.1.5.5.7.3.2) und Serverauthentifizierung (1.3.6.1.5.5.7.3.1).
 3. Der private Schlüssel für das Zertifikat ist auf dem *Zielknoten_ verfügbar.
 4. Der **Anbieter** für das Zertifikat muss „Microsoft RSA SChannel Cryptographic Provider“ sein.
 
>**Empfohlene bewährte Methode:** Sie können ein Zertifikat, das die Schlüsselverwendung „Digitale Signatur“ oder eine der „Authentifizierungs-EKU enthält, zwar verwenden, dadurch kann der die Verschlüsselungsschlüssel allerdings leichter missbraucht werden und ist anfälliger für Angriffe. Es empfiehlt sich daher, ein Zertifikat ohne diese Schlüsselverwendung und EKUs zu verwenden, das speziell zum Sichern von DSC-Anmeldeinformationen erstellt wurde.
  
Alle vorhandenen Zertifikate auf dem _Zielknoten_, die diese Kriterien erfüllen, können zum Absichern von DSC-Anmeldeinformationen verwendet werden.

## Erstellen von Zertifikaten

Es gibt zwei Ansätze, die Sie ergreifen können, um das erforderliche Verschlüsselungszertifikat (Paar aus öffentlichem/privatem Schlüssel) zu erstellen und zu verwenden .

1. Sie können es auf dem **Zielknoten** erstellen und nur den öffentlichen Schlüssel auf den **Erstellungsknoten** exportieren.
2. Sie können es auf dem **Erstellungsknoten** erstellen und das gesamte Schlüsselpaar auf den **Zielknoten** exportieren.

Empfohlen wird Methode 1, weil der im MOF zum Entschlüsseln von Anmeldeinformationen verwendete private Schlüssel immer auf dem Zielknoten verbleibt.


### Erstellen des Zertifikats auf dem Zielknoten

Der private Schlüssel muss geheim gehalten werden, weil er zum Entschlüsseln des MOF auf dem **Zielknoten** verwendet wird. Die einfachste Möglichkeit hierzu ist, das Zertifikat für private Schlüssel auf dem **Zielknoten** zu erstellen und das **Zertifikat für öffentliche Schlüssel** auf den Computer zu kopieren, der zum Erstellen der DSC-Konfiguration in einer MOF-Datei verwendet wird.
Im folgenden Beispiel
 1. ein Zertifikat auf dem **Zielknoten** erstellt.
 2. das Zertifikat für öffentliche Schlüssel auf den **Zielknoten** exportiert.
 3. das Zertifikat für öffentliche Schlüssel in den **my**-Zertifikatspeicher auf dem **Erstellungsknoten** importiert.

#### Auf dem Zielknoten: Erstellen und Exportieren des Zertifikats
>Erstellungsknoten: Windows Server 2016 und Windows 10

```powershell
# note: These steps need to be performed in an Administrator PowerShell session
$cert = New-SelfSignedCertificate -Type DocumentEncryptionCertLegacyCsp -DnsName 'DscEncryptionCert' -HashAlgorithm SHA256
# export the public key certificate
$cert | Export-Certificate -FilePath "$env:temp\DscPublicKey.cer" -Force
```
Einmal exportiert, müsste ```DscPublicKey.cer``` auf den **Erstellungsknoten** kopiert werden.

>Erstellungsknoten: Windows Server 2012 R2/Windows 8.1 und früher

Da das Cmdlet „New-SelfSignedCertificate“ unter niedrigeren Windows-Betriebssystemen als Windows 10 und Windows Server 2016 nicht den Parameter **Typ** unterstützt, ist eine alternative Methode zum Erstellen dieses Zertifikats unter diesen Betriebssystemen erforderlich.
In diesem Fall können Sie ```makecert.exe``` oder ```certutil.exe``` zum Erstellen des Zertifikats verwenden.

Eine alternative Methode besteht darin, [das Skript „New-SelfSignedCertificateEx.ps1“ aus dem Microsoft Script Center](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6) herunterzuladen und es stattdessen zum Erstellen des Zertifikats zu verwenden:
```powershell
# note: These steps need to be performed in an Administrator PowerShell session
# and in the folder that contains New-SelfSignedCertificateEx.ps1
. .\New-SelfSignedCertificateEx.ps1
New-SelfsignedCertificateEx `
    -Subject "CN=${ENV:ComputerName}" `
    -EKU 'Document Encryption' `
    -KeyUsage 'KeyEncipherment, DataEncipherment' `
    -SAN ${ENV:ComputerName} `
    -FriendlyName 'DSC Credential Encryption certificate' `
    -Exportable `
    -StoreLocation 'LocalMachine' `
    -StoreName 'My' `
    -KeyLength 2048 `
    -ProviderName 'Microsoft Enhanced Cryptographic Provider v1.0' `
    -AlgorithmName 'RSA' `
    -SignatureAlgorithm 'SHA256'
# Locate the newly created certificate
$Cert = Get-ChildItem -Path cert:\LocalMachine\My `
    | Where-Object {
        ($_.FriendlyName -eq 'DSC Credential Encryption certificate') `
        -and ($_.Subject -eq "CN=${ENV:ComputerName}")
    } | Select-Object -First 1
# export the public key certificate
$cert | Export-Certificate -FilePath "$env:temp\DscPublicKey.cer" -Force
```
Einmal exportiert, müsste ```DscPublicKey.cer``` auf den **Erstellungsknoten** kopiert werden.

#### Auf dem Erstellungsknoten: Importieren des öffentlichen Schlüssels des Zertifikats
```powershell
# Import to the my store
Import-Certificate -FilePath "$env:temp\DscPublicKey.cer" -CertStoreLocation Cert:\LocalMachine\My
```

### Erstellen des Zertifikats auf dem Erstellungsknoten
Alternativ kann das Verschlüsselungszertifikat auf dem **Erstellungsknoten** erstellt, mit dem **privaten Schlüssel** als PFX-Datei exportiert und dann auf dem **Zielknoten** importiert werden.
Dies ist die aktuelle Methode zur Implementierung der Verschlüsselung von DSC-Anmeldeinformationen unter _Nano Server_.
Obwohl die PFX-Datei mit einem Kennwort geschützt ist, sollte sie während der Übertragung gesichert werden.
Im folgenden Beispiel
 1. wird ein Zertifikat auf dem **Erstellungsknoten** erstellt.
 2. wird das Zertifikat, einschließlich des privaten Schlüssels, auf den **Erstellungsknoten** exportiert.
 3. wird der private Schlüssel vom **Erstellungsknoten** entfernt, aber das Zertifikat für den öffentlichen Schlüssel im **my**-Speicher beibehalten.
 4. wird das Zertifikat für den privaten Schlüssel in den Stammzertifikatspeicher auf dem **Zielknoten** importiert.
   - muss es dem Stammspeicher hinzugefügt werden, damit ihm der **Zielknoten** vertraut.

#### Auf dem Erstellungsknoten: Erstellen und Exportieren des Zertifikats
>Zielknoten: Windows Server 2016 und Windows 10

```powershell
# note: These steps need to be performed in an Administrator PowerShell session
$cert = New-SelfSignedCertificate -Type DocumentEncryptionCertLegacyCsp -DnsName 'DscEncryptionCert' -HashAlgorithm SHA256
# export the private key certificate
$mypwd = ConvertTo-SecureString -String "YOUR_PFX_PASSWD" -Force -AsPlainText
$cert | Export-PfxCertificate -FilePath "$env:temp\DscPrivateKey.pfx" -Password $mypwd -Force
# remove the private key certificate from the node but keep the public key certificate
$cert | Export-Certificate -FilePath "$env:temp\DscPublicKey.cer" -Force
$cert | Remove-Item -Force
Import-Certificate -FilePath "$env:temp\DscPublicKey.cer" -CertStoreLocation Cert:\LocalMachine\My
```
Einmal exportiert, müsste ```DscPrivateKey.cer``` auf den **Zielknoten** kopiert werden.

>Zielknoten: Windows Server 2012 R2/Windows 8.1 und früher

Da das Cmdlet „New-SelfSignedCertificate“ unter niedrigeren Windows-Betriebssystemen als Windows 10 und Windows Server 2016 nicht den Parameter **Typ** unterstützt, ist eine alternative Methode zum Erstellen dieses Zertifikats unter diesen Betriebssystemen erforderlich.
In diesem Fall können Sie ```makecert.exe``` oder ```certutil.exe``` zum Erstellen des Zertifikats verwenden.

Eine alternative Methode besteht darin, [das Skript „New-SelfSignedCertificateEx.ps1“ aus dem Microsoft Script Center](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6) herunterzuladen und es stattdessen zum Erstellen des Zertifikats zu verwenden:
```powershell
# note: These steps need to be performed in an Administrator PowerShell session
# and in the folder that contains New-SelfSignedCertificateEx.ps1
. .\New-SelfSignedCertificateEx.ps1
New-SelfsignedCertificateEx `
    -Subject "CN=${ENV:ComputerName}" `
    -EKU 'Document Encryption' `
    -KeyUsage 'KeyEncipherment, DataEncipherment' `
    -SAN ${ENV:ComputerName} `
    -FriendlyName 'DSC Credential Encryption certificate' `
    -Exportable `
    -StoreLocation 'LocalMachine' `
    -StoreName 'My' `
    -KeyLength 2048 `
    -ProviderName 'Microsoft Enhanced Cryptographic Provider v1.0' `
    -AlgorithmName 'RSA' `
    -SignatureAlgorithm 'SHA256'
# Locate the newly created certificate
$Cert = Get-ChildItem -Path cert:\LocalMachine\My `
    | Where-Object {
        ($_.FriendlyName -eq 'DSC Credential Encryption certificate') `
        -and ($_.Subject -eq "CN=${ENV:ComputerName}")
    } | Select-Object -First 1
# export the public key certificate
$mypwd = ConvertTo-SecureString -String "YOUR_PFX_PASSWD" -Force -AsPlainText
$cert | Export-PfxCertificate -FilePath "$env:temp\DscPrivateKey.pfx" -Password $mypwd -Force
# remove the private key certificate from the node but keep the public key certificate
$cert | Export-Certificate -FilePath "$env:temp\DscPublicKey.cer" -Force
$cert | Remove-Item -Force
Import-Certificate -FilePath "$env:temp\DscPublicKey.cer" -CertStoreLocation Cert:\LocalMachine\My
```

#### Auf dem Zielknoten: Importieren des privaten Schlüssels des Zertifikats als vertrauenswürdiger Stamm
```powershell
# Import to the root store so that it is trusted
$mypwd = ConvertTo-SecureString -String "YOUR_PFX_PASSWD" -Force -AsPlainText
Import-PfxCertificate -FilePath "$env:temp\DscPrivateKey.pfx" -CertStoreLocation Cert:\LocalMachine\Root -Password $mypwd > $null
```

## Konfigurationsdaten

Der „Configuration“-Datenblock definiert die betroffenen Zielknoten, ob die Anmeldeinformationen verschlüsselt werden oder nicht, die Verschlüsselungsmethode und andere Informationen. Weitere Informationen zum „Configuration“-Datenblock finden Sie unter [Trennen von Konfigurations- und Umgebungsdaten](configData.md).

Die Elemente im Zusammenhang mit der Verschlüsselung von Anmeldeinformationen für jeden Knoten konfiguriert werden können, sind:
* **NodeName**: Der Name des Zielknotens, für den die Verschlüsselung der Anmeldeinformationen konfiguriert wird.
* **PsDscAllowPlainTextPassword**: Legt fest, ob unverschlüsselte Anmeldeinformationen an diesen Knoten übergeben werden können. Dies ist **nicht zu empfehlen**.
* **Thumbprint**: Der Fingerabdruck des Zertifikats, das verwendet wird, um die Anmeldeinformationen in der DSC-Konfiguration auf dem _Zielknoten_ zu entschlüsseln. **Dieses Zertifikat muss in den Zertifikatspeicher des lokalen Computers auf dem Zielknoten vorhanden sein.**
* **CertificateFile**: Die Zertifikatsdatei (enthält nur den öffentlichen Schlüssel), die verwendet werden soll, um die Anmeldeinformationen für die _Zielknoten_ zu verschlüsseln. Dies muss eine Zertifikatdatei im DER-codierten binären X.509-Format oder im Base-64-codierten X.509-Format sein.

Dieses Beispiel zeigt einen „Configuration“-Datenblock, einen betroffenen Zielknoten namens „targetNode“, den Pfad zur Zertifikatdatei mit dem öffentlichen Schlüssel (namens „targetNode.cer“) und den Fingerabdruck des öffentlichen Schlüssels.

```powershell
$ConfigData= @{ 
    AllNodes = @(     
            @{  
                # The name of the node we are describing 
                NodeName = "targetNode" 

                # The path to the .cer file containing the 
                # public key of the Encryption Certificate 
                # used to encrypt credentials for this node 
                CertificateFile = "C:\publicKeys\targetNode.cer" 

         
                # The thumbprint of the Encryption Certificate 
                # used to decrypt the credentials on target node 
                Thumbprint = "AC23EA3A9E291A75757A556D0B71CBBF8C4F6FD8" 
            }; 
        );    
    }
```


## Konfigurationsskript

Verwenden Sie im Skript selbst den `PsCredential`-Parameter, um sicherzustellen, dass die Anmeldeinformationen so kurz wie möglich gespeichert werden. Wenn Sie das bereitgestellte Beispiel ausführen, werden Sie von DSC zum Eingeben von Anmeldeinformationen und anschließendem Verschlüsseln der MOF-Datei mithilfe der Zertifikatdatei aufgefordert, die dem Zielknoten im „Configuration“-Datenblock zugeordnet ist. Bei diesem Codebeispiel wird eine Datei aus einer geschützten Freigabe zu einem Benutzer kopiert.

```
configuration CredentialEncryptionExample 
{ 
    param( 
        [Parameter(Mandatory=$true)] 
        [ValidateNotNullorEmpty()] 
        [PsCredential] $credential 
        ) 
    

    Node $AllNodes.NodeName 
    { 
        File exampleFile 
        { 
            SourcePath = "\\Server\share\path\file.ext" 
            DestinationPath = "C:\destinationPath" 
            Credential = $credential 
        } 
    } 
}
```

## Einrichten der Entschlüsselung

Damit [`Start-DscConfiguration`](https://technet.microsoft.com/en-us/library/dn521623.aspx) funktionieren kann, müssen Sie den lokalen Konfigurations-Manager auf allen Zielknoten informieren, welches Zertifikat zum Entschlüsseln der Anmeldeinformationen verwendet werden soll. Die „CertificateID“-Ressource wird zum Überprüfen des Fingerabdrucks des Zertifikats verwendet. Diese Beispielfunktion findet das entsprechende lokale Zertifikat (Sie müssen ggf. eine Anpassung vornehmen, damit genau das gewünschte Zertifikat gefunden wird):

```powershell
# Get the certificate that works for encryption 
function Get-LocalEncryptionCertificateThumbprint 
{ 
    (dir Cert:\LocalMachine\my) | %{
        # Verify the certificate is for Encryption and valid 
        if ($_.PrivateKey.KeyExchangeAlgorithm -and $_.Verify()) 
        { 
            return $_.Thumbprint 
        } 
    } 
}
```

Mit dem über seinen Fingerabdruck bestimmten Zertifikat kann das Konfigurationsskript für das Verwenden des folgenden Werts aktualisiert werden:

```powershell
configuration CredentialEncryptionExample 
{ 
    param( 
        [Parameter(Mandatory=$true)] 
        [ValidateNotNullorEmpty()] 
        [PsCredential] $credential 
        ) 
    

    Node $AllNodes.NodeName 
    { 
        File exampleFile 
        { 
            SourcePath = "\\Server\share\path\file.ext" 
            DestinationPath = "C:\destinationPath" 
            Credential = $credential 
        } 
        
        LocalConfigurationManager 
        { 
             CertificateId = $node.Thumbprint 
        } 
    } 
}
```

## Ausführen der Konfiguration

An dieser Stellen können Sie die Konfiguration ausführen, woraufhin zwei Dateien ausgegeben werden:

 * Eine Datei des Typs „*.meta.mof“, die vom lokalen Konfigurations-Manager zum Entschlüsseln der Anmeldeinformationen mithilfe des Zertifikats verwendet wird, das sich im lokalen Computerspeicher befindet und von dessen Fingerabdruck bestimmt wird. [`Set-DscLocalConfigurationManager`](https://technet.microsoft.com/en-us/library/dn521621.aspx) installiert die *.meta.mof-Datei.
 * Eine MOF-Datei, die die Konfiguration tatsächlich anwendet. „Start-DscConfiguration“ wendet die Konfiguration an.

Mit diesen Befehlen werden diese Schritte ausgeführt:

```powershell
Write-Host "Generate DSC Configuration..."
CredentialEncryptionExample -ConfigurationData $ConfigData -OutputPath .\CredentialEncryptionExample

Write-Host "Setting up LCM to decrypt credentials..."
Set-DscLocalConfigurationManager .\CredentialEncryptionExample -Verbose 
 
Write-Host "Starting Configuration..."
Start-DscConfiguration .\CredentialEncryptionExample -wait -Verbose
```

In diesem Beispiel wird die DSC-Konfiguration per Push auf den Zielknoten übertragen.
Die DSC-Konfiguration kann auch unter Verwendung eines DSC-Pullservers, sofern verfügbar, angewendet werden.

Weitere Informationen zum Anwenden von DSC-Konfigurationen unter Verwendung eines DSC-Pullservers finden Sie unter [Einrichten eines DSC-Pullclients](pullClient.md).

## Beispiel für das Modul zum Verschlüsseln von Anmeldeinformationen

Es folgt ein vollständiges Beispiel, das alle diese Schritte enthält, sowie ein Hilfs-Cmdlet zum Exportieren und Kopieren der öffentlichen Schlüssel:

```powershell
# A simple example of using credentials
configuration CredentialEncryptionExample
{
    param(
        [Parameter(Mandatory=$true)]
        [ValidateNotNullorEmpty()]
        [PsCredential] $credential
        )
    

    Node $AllNodes.NodeName
    {
        File exampleFile
        {
            SourcePath = "\\server\share\file.txt"
            DestinationPath = "C:\Users\user"
            Credential = $credential
        }
        
        LocalConfigurationManager
        {
            CertificateId = $node.Thumbprint
        }
    }
}

# A Helper to invoke the configuration, with the correct public key 
# To encrypt the configuration credentials
function Start-CredentialEncryptionExample
{
    [CmdletBinding()]
    param ($computerName)


    [string] $thumbprint = Get-EncryptionCertificate -computerName $computerName -Verbose
    Write-Verbose "using cert: $thumbprint"

    $certificatePath = join-path -Path "$env:SystemDrive\$script:publicKeyFolder" -childPath "$computername.EncryptionCertificate.cer"         

    $ConfigData=    @{
        AllNodes = @(     
                        @{  
                            # The name of the node we are describing
                            NodeName = "$computerName"

                            # The path to the .cer file containing the
                            # public key of the Encryption Certificate
                            CertificateFile = "$certificatePath"

                            # The thumbprint of the Encryption Certificate
                            # used to decrypt the credentials
                            Thumbprint = $thumbprint
                        };
                    );    
    }

    Write-Verbose "Generate DSC Configuration..."
    CredentialEncryptionExample -ConfigurationData $ConfigData -OutputPath .\CredentialEncryptionExample `
        -credential (Get-Credential -UserName "$env:USERDOMAIN\$env:USERNAME" -Message "Enter credentials for configuration") 

    Write-Verbose "Setting up LCM to decrypt credentials..."
    Set-DscLocalConfigurationManager .\CredentialEncryptionExample -Verbose 

    Write-Verbose "Starting Configuration..."
    Start-DscConfiguration .\CredentialEncryptionExample -wait -Verbose

}


#region HelperFunctions

# The folder name for the exported public keys
$script:publicKeyFolder = "publicKeys"

# Get the certificate that works for encryptions
function Get-EncryptionCertificate
{
    [CmdletBinding()]
    param ($computerName)
    $returnValue= Invoke-Command -ComputerName $computerName -ScriptBlock {
            $certificates = dir Cert:\LocalMachine\my

            $certificates | %{
                    # Verify the certificate is for Encryption and valid
                    if ($_.PrivateKey.KeyExchangeAlgorithm -and $_.Verify())
                    {
                        # Create the folder to hold the exported public key
                        $folder= Join-Path -Path $env:SystemDrive\ -ChildPath $using:publicKeyFolder
                        if (! (Test-Path $folder))
                        {
                            md $folder | Out-Null
                        }

                        # Export the public key to a well known location
                        $certPath = Export-Certificate -Cert $_ -FilePath (Join-Path -path $folder -childPath "EncryptionCertificate.cer") 

                        # Return the thumbprint, and exported certificate path
                        return @($_.Thumbprint,$certPath);
                    }
                  }
        }
    Write-Verbose "Identified and exported cert..."
    # Copy the exported certificate locally
    $destinationPath = join-path -Path "$env:SystemDrive\$script:publicKeyFolder" -childPath "$computername.EncryptionCertificate.cer"
    Copy-Item -Path (join-path -path \\$computername -childPath $returnValue[1].FullName.Replace(":","$"))  $destinationPath | Out-Null

    # Return the thumbprint
    return $returnValue[0]
}

Start-CredentialEncryptionExample
```




<!--HONumber=Oct16_HO1-->


