---
description: 
manager: dongill
ms.topic: article
author: jpjofre
ms.prod: powershell
keywords: powershell,cmdlet,jea
ms.date: 2016-06-22
title: "Erstellen eines Domänencontrollers"
ms.technology: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 8473eb668e4da5bab01c2f2b7647cbced413bd22

---

### Erstellen eines Domänencontrollers

Im vorliegenden Dokument wird davon ausgegangen, dass Ihr Computer in eine Domäne eingebunden ist.
Wenn Sie derzeit nicht über eine Domäne verfügen, in die der Computer eingebunden werden kann, hilft Ihnen dieser Abschnitt dabei, mithilfe von DSC schnell einen Domänencontroller einzurichten.

#### Voraussetzungen

1.  Der Computer befindet sich in einem internen Netzwerk.
2.  Der Computer ist nicht in eine vorhandene Domäne eingebunden.
3.  Auf dem Computer wird Windows Server 2016 ausgeführt, oder es ist WMF 5.0 installiert.

#### Installieren von xActiveDirectory
Wenn Ihr Computer über eine aktive Internetverbindung verfügt, führen Sie folgenden Befehl in einem PowerShell-Fenster mit erhöhten Rechten aus:
```PowerShell
Install-Module xActiveDirectory -Force
```
Wenn Sie nicht über eine Internetverbindung verfügen, installieren Sie xActiveDirectory auf einem anderen Computer, und kopieren Sie den xActiveDirectory-Ordner in das Verzeichnis „C:\Programme\WindowsPowerShell\Modules“ auf Ihrem Computer.

Um zu überprüfen, ob die Installation erfolgreich war, führen Sie folgenden Befehl aus:
```PowerShell
Get-Module xActiveDirectory -ListAvailable
```

#### Einrichten einer Domäne mit DSC
Kopieren Sie das folgende Skript in PowerShell, um Ihren Computer als Domänencontroller in einer neuen Domäne einzurichten.
**HINWEIS DES AUTORS: ES IST EIN BEKANNTES PROBLEM MIT DEN ANMELDEINFORMATIONEN NICHT VERWENDET WIRD.  UM SICHER ZU SEIN, VERGESSEN SIE NICHT IHR KENNWORT DES LOKALEN ADMINISTRATORS.**

```PowerShell
Set-Item WSMan:\localhost\Client\TrustedHosts -Value $env:COMPUTERNAME -Force

# This "MetaConfiguration" sets the DSC Engine to automatically reboot if required
[DscLocalConfigurationManager()]
Configuration MetaConfiguration
{
    Node $env:Computername
    {
        Settings
        {
            RebootNodeIfNeeded = $true
        }
    }

}

MetaConfiguration
# Apply the MetaConfiguration
Set-DscLocalConfigurationManager .\MetaConfiguration

# Configure a domain controller of a new "Contoso" domain
configuration DomainController
{
    param
    (
        $node,
        $cred
    )
    Import-DscResource -ModuleName xActiveDirectory

    Node $node
    {
        WindowsFeature ADDS
        {
            Ensure = 'Present'
            Name = 'AD-Domain-Services'
        }

        xADDomain Contoso
        {
            DomainName = 'contoso.com'
            DomainAdministratorCredential = $cred
            SafemodeAdministratorPassword = $cred
            DependsOn = '[WindowsFeature]ADDS'
        }

        file temp
        {
            DestinationPath = 'C:\temp.txt'
            Contents = 'Domain has been created'
            DependsOn = '[xADDomain]Contoso'
        }
    }
}

$ConfigData = @{
    AllNodes = @(
        @{
            NodeName = $env:Computername
            PSDscAllowPlainTextPassword = $true
        }
    )
}

# Enter your desired password for the domain administrator (note, this will be stored as plain text)
DomainController -cred (Get-Credential -Message "Enter desired credential for domain administrator") -node $env:Computername -configurationData $ConfigData

# Apply the configuration to create the domain controller
Start-DSCConfiguration -path .\DomainController -ComputerName $env:Computername -Wait -Force -Verbose
```
Ihr Computer wird mehrmals neu gestartet.
Sobald Sie eine Datei namens „C:\temp.txt“ mit dem Inhalt „Domain has been created.“ sehen, wissen Sie, dass der Vorgang abgeschlossen ist.

#### Einrichten von Benutzern und Gruppen
Mit den folgenden Befehlen richten Sie eine Gruppe „Helpdesk“ und eine Gruppe „Operator“ sowie jeweils einen entsprechenden Benutzer ohne Administratorrechte in Ihrer Domäne ein, der Mitglied der jeweiligen Gruppe ist.
```PowerShell
# Make Groups
$NonAdminOperatorGroup = New-ADGroup -Name "JEA_NonAdmin_Operator" -GroupScope DomainLocal -PassThru
$NonAdminHelpDeskGroup = New-ADGroup -Name "JEA_NonAdmin_HelpDesk" -GroupScope DomainLocal -PassThru
$TestGroup = New-ADGroup -Name "Test_Group" -GroupScope DomainLocal -PassThru

# Make Users
$OperatorUser = New-ADUser -Name "OperatorUser" -AccountPassword (ConvertTo-SecureString 'pa$$w0rd' -AsPlainText -Force) -PassThru
Enable-ADAccount -Identity $OperatorUser

$HelpDeskUser = New-ADUser -name "HelpDeskUser" -AccountPassword (ConvertTo-SecureString 'pa$$w0rd' -AsPlainText -Force) -PassThru
Enable-ADAccount -Identity $HelpDeskUser

# Add Users to Groups
Add-ADGroupMember -Identity $NonAdminOperatorGroup -Members $OperatorUser
Add-ADGroupMember -Identity $NonAdminHelpDeskGroup -Members $HelpDeskUser
```




<!--HONumber=Oct16_HO1-->


