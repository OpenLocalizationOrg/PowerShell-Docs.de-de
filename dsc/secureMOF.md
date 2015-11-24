#Sichern die MOF-Datei

> Gilt für: WindowsPowerShell 4.0, WindowsPowerShell 5.0

DSC teilt den Zielknoten welche Konfiguration sie durch Senden einer MOF-Datei mit diesen Informationen für jeden Knoten, in dem die gewünschte Konfiguration implementiert die lokale Configuration Manager (LCM) verfügen. Diese Datei enthält die Details der Konfiguration ist es wichtig, um es zu schützen. Zu diesem Zweck können Sie den lokalen Konfigurations-Manager überprüft die Anmeldeinformationen eines Benutzers festlegen. Dieses Thema beschreibt, wie diese Anmeldeinformationen sicher an den Zielknoten übertragen mit Zertifikaten verschlüsseln.

##Erforderliche Komponenten

Um die Anmeldeinformationen für das Sichern einer DSC-Konfigurations erfolgreich zu verschlüsseln, stellen Sie sicher, dass Sie über Folgendes verfügen:

* **Eine Möglichkeit zum Ausstellen und Verteilen von Zertifikaten**. In diesem Thema und seinen Beispielen wird davon ausgegangen, dass Sie Active Directory-Zertifizierungsstelle verwenden. Weitere Informationen zu Active Directory Certificate Services, finden Sie unter [Übersicht über Active Directory Certificate Services](https://technet.microsoft.com/library/hh831740.aspx) und [Active Directory-Zertifikatdienste in Windows Server 2008](https://technet.microsoft.com/windowsserver/dd448615.aspx).
* **Administrative den Zugriff auf den Zielknoten oder Knoten**.
* **Jeder Zielknoten hat ein Verschlüsselung kann seine persönlichen Speicher gespeichert**. In Windows PowerShell ist der Pfad zum Speicher Cert: \LocalMachine\My. In den Beispielen in diesem Thema verwenden Sie die Vorlage "Arbeitsstationsauthentifizierung" (zusammen mit anderen Zertifikatvorlagen) finden Sie unter [Standardzertifikatvorlagen] (https://technet.microsoft.com/library/cc740061 (v=WS.10).aspx).
* Wenn Sie diese Konfiguration auf einem anderen Computer als dem Zielknoten ausführen **Exportieren Sie den öffentlichen Schlüssel des Zertifikats**, und importieren Sie es auf dem Computer führen Sie die Konfiguration aus. Stellen Sie sicher, dass Sie nur Exportieren der **öffentlichen** Schlüssel, den privaten Schlüssel schützen.

##Der Prozess

1. Richten Sie die Zertifikate, Schlüssel und Fingerabdrücke, um sicherzustellen, dass jede Zielknoten Kopien des Zertifikats und der Konfigurationscomputer über den öffentlichen Schlüssel und Fingerabdruck.
1. Erstellen Sie einen Konfiguration Datenblock, der den Pfad und den Fingerabdruck des öffentlichen Schlüssels enthält.
1. Erstellen Sie ein Konfigurationsskript, die die gewünschte Konfiguration für den Zielknoten definiert und richtet die Zielknoten Entschlüsselung durch die lokale Konfiguration Befehle-Manager, um die Konfigurationsdaten, die mit dem Zertifikat und den dazugehörigen Fingerabdruck zu entschlüsseln.
1. Führen Sie die Konfiguration, die die lokale Configuration Manager-Einstellungen festlegen und die DSC-Konfiguration zu starten.

##Konfigurationsdaten

Datenblock Konfiguration definiert, welche Zielknoten auf, ob angewendet werden oder nicht, um die Anmeldeinformationen, die Mittel der Verschlüsselung und andere Informationen zu verschlüsseln. Weitere Informationen über die Konfiguration Datenblock, finden Sie unter [Trennen Konfigurations- und Umgebungsdaten](configData.md).

Dieses Beispiel zeigt einen Konfiguration Datenblock, der benannte TargetNode, den Pfad zur Datei Zertifikat für öffentliche Schlüssel (mit dem Namen "targetNode.cer") und den Fingerabdruck des öffentlichen Schlüssels fungieren Zielknoten angibt.

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

##Skript für Konfiguration

Verwenden Sie das Skript selbst, die `PsCredential` Parameter, um sicherzustellen, dass die Anmeldeinformationen für kürzester Zeit gespeichert werden. Wenn Sie das bereitgestellte Beispiel ausführen, wird DSC aufgefordert, Anmeldeinformationen und Verschlüsseln Sie die MOF-Datei mit der CertificateFile, der den Zielknoten im Datenblock Konfiguration zugeordnet ist. In diesem Codebeispiel wird kopiert eine Datei von einer Freigabe, die gesichert wurden, an einen Benutzer.

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

##Einrichten der Entschlüsselung

Vor dem `Start DscConfiguration` arbeiten können, müssen Sie den lokalen Konfigurations-Manager auf jedem Zielknoten das Zertifikat zum Entschlüsseln der Anmeldeinformationen mitteilen CertificateID Ressource verwenden, um den Fingerabdruck des Zertifikats zu überprüfen. Diese Beispielfunktion findet das richtige Zertifikat mit lokale (möglicherweise müssen Sie ihn anpassen, damit das genaue Zertifikat gefunden wird, die, das Sie verwenden möchten):

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

Mit dem Zertifikat mit seinem Fingerabdruck identifiziert wird kann das Skript aktualisiert werden, um den Wert zu verwenden:

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

##Die Konfiguration ausgeführt wird

An diesem Punkt können Sie die Konfiguration ausführen, die zwei Dateien ausgegeben wird:

* A *. meta.mof-Datei, die zum Entschlüsseln der Anmeldeinformationen, die mit dem Zertifikat, das auf dem lokalen Computerspeicher gespeichert und mit seinem Fingerabdruck identifiziert den lokalen Konfigurations-Manager konfiguriert. Set-DscLocalConfigurationManager gilt die *. meta.mof-Datei.
* Eine MOF-Datei, die tatsächlich die Konfiguration angewendet wird. Start-DscConfiguration gilt die Konfiguration.

Diese Befehle werden diese Schritte ausführen:

```powershell
Write-Host "Generate DSC Configuration..."
CredentialEncryptionExample -ConfigurationData $ConfigData -OutputPath .\CredentialEncryptionExample

Write-Host "Setting up LCM to decrypt credentials..."
Set-DscLocalConfigurationManager .\CredentialEncryptionExample -Verbose 

Write-Host "Starting Configuration..."
Start-DscConfiguration .\CredentialEncryptionExample -wait -Verbose
```

Hier ist ein vollständiges Beispiel, das alle diese Schritte sowie eine Helper-Cmdlet, das exportiert und kopiert die öffentlichen Schlüssel enthält:

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
```



