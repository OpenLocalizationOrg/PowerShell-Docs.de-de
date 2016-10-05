---
title: Einrichten eines DSC-SMB-Pullservers
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 6ab0c155299b2baaf901646f4fdd69133e9ed0ca

---

# Einrichten eines DSC-SMB-Pullservers

>Gilt für: Windows PowerShell 4.0, Windows PowerShell 5.0

Ein DSC-[SMB](https://technet.microsoft.com/en-us/library/hh831795.aspx)-Pullserver ist eine SMB-Dateifreigabe, mit der DSC-Konfigurationsdateien und/oder DSC-Ressourcen für Zielknoten zur Verfügung gestellt werden, wenn sie von diesen Knoten angefordert werden.

Zum Verwenden eines SMB-Pullservers für DSC müssen Sie folgende Schritte ausführen:
- Einrichten einer SMB-Dateifreigabe auf einem Server mit PowerShell 4.0 oder höher
- Konfigurieren eines Clients mit PowerShell 4.0 oder höher zum Abrufen von dieser SMB-Freigabe

## Verwenden der xSmbShare-Ressource zum Erstellen einer SMB-Dateifreigabe

Es gibt zahlreiche Methoden zum Einrichten einer SMB-Dateifreigabe. Im Folgenden wird die Methode unter Verwendung von DSC beschrieben.

### Installieren der Ressource „xSmbShare“

Rufen Sie das Cmdlet [Install-Module](https://technet.microsoft.com/en-us/library/dn807162.aspx) auf, um das Modul **xSmbShare** zu installieren.
>**Hinweis**: **Install-Module** ist im Modul **PowerShellGet** enthalten, das Bestandteil von PowerShell 5.0 ist. Das Modul **PowerShellGet** für PowerShell 3.0 und 4.0 können Sie unter [PowerShell-Module „PackageManagement“ – Vorschau](https://www.microsoft.com/en-us/download/details.aspx?id=49186) herunterladen. **xSmbShare** enthält die DSC-Ressource **xSmbShare**, mit der Sie eine SMB-Dateifreigabe erstellen können.

### Erstellen des Verzeichnisses und der Dateifreigabe

Die folgende Konfiguration verwendet die [File](fileResource.md)-Ressource zum Erstellen des Verzeichnisses für die Freigabe und die **xSmbShare**-Ressource zum Einrichten der SMB-Freigabe:

```powershell
Configuration SmbShare {

Import-DscResource -ModuleName PSDesiredStateConfiguration
Import-DscResource -ModuleName xSmbShare
 
    Node localhost {
 
        File CreateFolder {
 
            DestinationPath = 'C:\DscSmbShare'
            Type = 'Directory'
            Ensure = 'Present'
 
        }
 
        xSMBShare CreateShare {
 
            Name = 'DscSmbShare'
            Path = 'C:\DscSmbShare'
            FullAccess = 'admininstrator'
            ReadAccess = 'myDomain\Contoso-Server$'
            FolderEnumerationMode = 'AccessBased'
            Ensure = 'Present'
            DependsOn = '[File]CreateFolder'
 
        }
        
    }
 
}
```

Die Konfiguration erstellt das Verzeichnis `C:\DscSmbShare`, wenn es noch nicht vorhanden ist, und verwendet dieses Verzeichnis anschließend als SMB-Dateifreigabe. Jedes Konto, das in die Dateifreigabe schreiben oder Elemente daraus löschen können muss, sollte **FullAccess** erhalten, und allen Clientknoten, die Konfigurationen und/oder DSC-Ressourcen aus dieser Freigabe erhalten, **ReadAccess** gewährt werden (da DSC standardmäßig als Systemkonto ausgeführt wird, daher muss der Computer selbst auf die Freigabe zugreifen können).


### Gewähren von Dateisystemzugriff für den Pullclient

Indem Sie einem Clientknoten **ReadAccess** gewähren, kann dieser Knoten auf die SMB-Freigabe zugreifen, aber nicht auf Dateien oder Ordner innerhalb der Freigabe. Sie müssen Clientknoten explizit Zugriff auf den SMB-Freigabeordner und dessen Unterordner gewähren. In DSC erfolgt dies mithilfe der Ressource **cNtfsPermissionEntry**, die im Modul [CNtfsAccessControl](https://www.powershellgallery.com/packages/cNtfsAccessControl/1.2.0) enthalten ist. Die folgende Konfiguration fügt einen **cNtfsPermissionEntry**-Block hinzu, der dem Pullclient „ReadAndExecute“-Zugriff gewährt:

```powershell
Configuration DSCSMB {

Import-DscResource -ModuleName PSDesiredStateConfiguration
Import-DscResource -ModuleName xSmbShare
Import-DscResource -ModuleName cNtfsAccessControl
 
    Node localhost {
 
        File CreateFolder {
 
            DestinationPath = 'DscSmbShare'
            Type = 'Directory'
            Ensure = 'Present'
 
        }
 
        xSMBShare CreateShare {
 
            Name = 'DscSmbShare'
            Path = 'DscSmbShare'
            FullAccess = 'administrator'
            ReadAccess = 'myDomain\Contoso-Server$'
            FolderEnumerationMode = 'AccessBased'
            Ensure = 'Present'
            DependsOn = '[File]CreateFolder'
 
        }

        cNtfsPermissionEntry PermissionSet1 {
            
        Ensure = 'Present'
        Path = 'C:\DSCSMB'
        Principal = 'myDomain\Contoso-Server$'
        AccessControlInformation = @(
            cNtfsAccessControlInformation
            {
                AccessControlType = 'Allow'
                FileSystemRights = 'ReadAndExecute'
                Inheritance = 'ThisFolderSubfoldersAndFiles'
                NoPropagateInherit = $false
            }
        )
        DependsOn = '[File]CreateFolder'
        
        }
 
        
    }
 
}
```

## Platzieren von Konfigurationen und Ressourcen

Speichern Sie alle MOF-Konfigurationsdateien und/oder DSC-Ressourcen, die von Clientknoten per Pull abgerufen werden sollen, im SMB-Freigabeordner.

Alle MOF-Konfigurationsdateien müssen den Namen _ConfigurationID_.mof haben, wobei _ConfigurationID_ der Wert der **ConfigurationID**-Eigenschaft des LCM des Zielknotens ist. Weitere Informationen zum Einrichten von Pullclients finden Sie unter [Einrichten eines DSC-Pullclients mithilfe einer Konfigurations-ID](pullClientConfigID.md).

>**Hinweis:** Sie müssen Konfigurations-IDs verwenden, wenn Sie einen SMB-Pullserver verwenden. Konfigurationsnamen werden für SMB nicht unterstützt.

Alle vom Client benötigten Ressourcen müssen in den SMB-Freigabeordner als archivierte `.zip`-Dateien abgelegt werden.  

## Erstellen der MOF-Prüfsumme
Eine MOF-Konfigurationsdatei muss einer Prüfsummendatei zugeordnet werden, damit ein LCM auf einem Zielknoten die Konfiguration überprüfen kann. Um eine Prüfsumme zu erstellen, rufen Sie das Cmdlet [New-DSCCheckSum](https://technet.microsoft.com/en-us/library/dn521622.aspx) auf. Das Cmdlet verwendet einen **Path**-Parameter, der den Ordner angibt, in dem sich die MOF-Konfigurationsdatei befindet. Das Cmdlet erstellt eine Prüfsummendatei mit dem Namen `ConfigurationMOFName.mof.checksum`, wobei `ConfigurationMOFName` der Name der MOF-Konfigurationsdatei ist. Wenn in dem angegebenen Ordner mehrere MOF-Konfigurationsdateien vorhanden sind, wird für jede Konfiguration im Ordner eine Prüfsumme erstellt.

Die Prüfsummendatei muss sich im gleichen Verzeichnis wie die MOF-Konfigurationsdatei befinden (standardmäßig `$env:PROGRAMFILES\WindowsPowerShell\DscService\Configuration`). Sie muss den gleichen Namen mit der Erweiterung `.checksum` haben.

>**Hinweis**: Wenn Sie die MOF-Konfigurationsdatei in irgendeiner Weise ändern, müssen Sie auch die Prüfsummendatei neu erstellen.

## Bestätigungen

Besonderer Dank geht an folgende Personen:

- Mike F. Robbins, dessen Beiträge zur Verwendung von SMB für DSC beim Zusammenstellen des Inhalts in diesem Thema geholfen haben. Seinen Blog finden Sie unter [Mike F Robbins](http://mikefrobbins.com/).
- Serge Nikalaichyk, der das Modul **cNtfsAccessControl** erstellt hat. Die Quelle für dieses Modul finden Sie unter „https://github.com/SNikalaichyk/cNtfsAccessControl“.

## Siehe auch
- [Windows PowerShell DSC – Übersicht](overview.md)
- [Inkraftsetzung von Konfigurationen](enactingConfigurations.md)
- [Einrichten eines Pullclients mithilfe einer Konfigurations-ID](pullClientConfigID.md)

 



<!--HONumber=Oct16_HO1-->


