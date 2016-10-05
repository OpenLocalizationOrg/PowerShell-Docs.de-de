---
title: "Ausführen von DSC mit Benutzeranmeldeinformationen"
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: dbe2c1ca2fb7dd65b49876f3bee6752ec9a24d6b

---

# Ausführen von DSC mit Benutzeranmeldeinformationen 

> Gilt für: Windows PowerShell 5.0

Sie können eine DSC-Ressource unter einem angegebenen Satz von Anmeldeinformationen ausführen, indem Sie die automatische Eigenschaft **PsDscRunAsCredential** in der Konfiguration verwenden. Standardmäßig führt DSC jede Ressource unter dem Systemkonto aus. Es gibt Situationen, in denen DSC unter einem Benutzerkonto ausgeführt werden muss, z. B. beim Installieren von MSI-Paketen in einem bestimmten Benutzerkontext, beim Festlegen von Registrierungsschlüsseln eines Benutzers, beim Zugriff auf ein bestimmtes lokales Verzeichnis eines Benutzers oder beim Zugriff auf eine Netzwerkfreigabe.

Jede DSC-Ressource verfügt über eine **PsDscRunAsCredential**-Eigenschaft, die auf beliebige Anmeldeinformationen festgelegt werden kann (ein [PSCredential](https://msdn.microsoft.com/en-us/library/ms572524(v=VS.85).aspx)-Objekt).
Die Anmeldeinformationen können als Wert der Eigenschaft in der Konfiguration hartcodiert werden, oder Sie können den Wert auf [Get-Credential](https://technet.microsoft.com/en-us/library/hh849815.aspx) festlegen, wodurch der Benutzer beim Kompilieren der Konfiguration zur Eingabe der Anmeldeinformationen aufgefordert wird (Informationen zum Kompilieren von Konfigurationen finden Sie unter [Konfigurationen](configurations.md).

>**Hinweis**: Die Eigenschaft **PsDscRunAsCredential** ist in PowerShell 4.0 nicht verfügbar.

Im folgenden Beispiel wird **Get-Credential** verwendet, um den Benutzer zur Eingabe von Anmeldeinformationen aufzufordern. Die Ressource [Registry](registryResource.md) wird verwendet, um den Registrierungsschlüssel zu ändern, der die Hintergrundfarbe für das Windows-Eingabeaufforderungsfenster angibt.

```powershell
Configuration ChangeCmdBackGroundColor    
{
    Import-DscResource -ModuleName PSDesiredStateConfiguration

    Node $AllNodes.NodeName
    {
        Registry CmdPath
        {
            Key                  = 'HKEY_CURRENT_USER\SOFTWARE\Microsoft\Command Processor'
            ValueName            = 'DefaultColor'
            ValueData            = '1F'
            ValueType            = 'DWORD'
            Ensure               = 'Present'
            Force                = $true
            Hex                  = $true
            PsDscRunAsCredential = Get-Credential
        }
    }                   
}

$configData = @{
    AllNodes = @(
        @{
            NodeName             = 'localhost';
            PSDscAllowDomainUser = $true
            CertificateFile      = 'C:\publicKeys\targetNode.cer'
            Thumbprint           = '7ee7f09d-4be0-41aa-a47f-96b9e3bdec25'
        }
    )
}

ChangeCmdBackGroundColor -ConfigurationData $configData
```
>**Hinweis**: In diesem Beispiel wird vorausgesetzt, dass Sie über ein gültiges Zertifikat unter `C:\publicKeys\targetNode.cer` verfügen und dass der Fingerabdruck dieses Zertifikats der angezeigte Wert ist.
>Informationen zum Verschlüsseln von Anmeldeinformationen in DSC MOF-Konfigurationsdateien finden Sie unter [Schützen der MOF-Datei](secureMOF.md).




<!--HONumber=Oct16_HO1-->


