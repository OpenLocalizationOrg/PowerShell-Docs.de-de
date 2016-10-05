---
description: 
manager: dongill
ms.topic: article
author: jpjofre
ms.prod: powershell
keywords: powershell,cmdlet,jea
ms.date: 2016-06-22
title: Voraussetzungen
ms.technology: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: ac9231a475ba84e9051bbd06a65f3f20c9e49846

---

# Voraussetzungen

## Anfangszustand
Bevor Sie mit diesem Abschnitt beginnen, stellen Sie Folgendes sicher:

1. JEA ist auf Ihrem System verfügbar. Lesen Sie die [Infodatei](./README.md), um Informationen zu den derzeit unterstützten Betriebssystemen und erforderlichen Downloads zu erhalten.
2. Sie verfügen über Administratorrechte auf dem Computer, auf dem Sie JEA ausprobieren.
3. Der Computer ist in die Domäne eingebunden.
Falls Sie noch nicht über eine Domäne verfügen, finden Sie im Abschnitt [Erstellen eines Domänencontrollers](#creating-a-domain-controller) Informationen zum schnellen Erstellen einer neuen Domäne auf einem Server.

## Aktivieren von PowerShell-Remoting
Die Verwaltung mit JEA erfolgt über PowerShell-Remoting.
Führen Sie Folgendes in einem PowerShell-Administratorfenster aus, um sicherzustellen, dass PowerShell-Remoting aktiviert und ordnungsgemäß konfiguriert ist:

```PowerShell
Enable-PSRemoting
```

Wenn Sie mit PowerShell-Remoting noch nicht vertraut sind, empfiehlt es sich, `Get-Help about_Remote` auszuführen, um Informationen zu diesem wichtigen und grundlegenden Konzept zu erhalten.

## Identifizieren der Benutzer und Gruppen
Um JEA in Aktion zu erleben, müssen Sie die Benutzer und Gruppen ohne Administratorrechte identifizieren, die Sie in diesem Leitfaden verwenden werden.

Wenn Sie eine vorhandene Domäne verwenden, identifizieren oder erstellen Sie einige Benutzer und Gruppen ohne solche Rechte.
Sie werden diesen Benutzern ohne Administratorrechte Zugriff auf JEA gewähren.
Immer, wenn Sie die `$NonAdministrator`-Variable oben in einem Skript sehen, weisen Sie diese Ihren ausgewählten Benutzern oder Gruppen ohne Administratorrechte zu.

Wenn Sie eine neue Domäne erstellt haben, ist die Aufgabe wesentlich leichter.
Verwenden Sie den Abschnitt [Set Up Users and Groups](creating-a-domain-controller.md#set-up-users-and-groups) (Einrichten von Benutzern und Gruppen) im Anhang, um Benutzer und Gruppen ohne Administratorrechte zu erstellen.
Die in diesem Abschnitt erstellten Gruppen weisen standardmäßig den Wert `$NonAdministrator` auf.

## Einrichten der Rollenfunktionsdatei für die Wartungsrolle
Führen Sie die folgenden Befehle in PowerShell aus, um die Rollenfunktionsdatei für die Demo zu erstellen, die Sie im nächsten Abschnitt benötigen.
Im späteren Verlauf dieses Leitfadens werden Sie erfahren, wozu diese Datei dient.

```PowerShell
# Fields in the role capability
$MaintenanceRoleCapabilityCreationParams = @{
    Author = 'Contoso Admin'
    CompanyName = 'Contoso'
    VisibleCmdlets = 'Restart-Service'
    FunctionDefinitions =
            @{ Name = 'Get-UserInfo'; ScriptBlock = { $PSSenderInfo } }
}

# Create the demo module, which will contain the maintenance Role Capability File
New-Item -Path "$env:ProgramFiles\WindowsPowerShell\Modules\Demo_Module" -ItemType Directory
New-ModuleManifest -Path "$env:ProgramFiles\WindowsPowerShell\Modules\Demo_Module\Demo_Module.psd1"
New-Item -Path "$env:ProgramFiles\WindowsPowerShell\Modules\Demo_Module\RoleCapabilities" -ItemType Directory

# Create the Role Capability file
New-PSRoleCapabilityFile -Path "$env:ProgramFiles\WindowsPowerShell\Modules\Demo_Module\RoleCapabilities\Maintenance.psrc" @MaintenanceRoleCapabilityCreationParams
```

## Erstellen und Registrieren der Sitzungskonfigurationsdatei für die Demo
Führen Sie die folgenden Befehle aus, um die Sitzungskonfigurationsdatei für die Demo zu erstellen und zu registrieren, die Sie im nächsten Abschnitt benötigen.
Im späteren Verlauf dieses Leitfadens werden Sie erfahren, wozu diese Datei dient.

```PowerShell
# Determine domain
$domain = (Get-CimInstance -ClassName Win32_ComputerSystem).Domain

# Replace with your non-admin group name
$NonAdministrator = "$domain\JEA_NonAdmin_Operator"

# Specify the settings for this JEA endpoint
# Note: You will not be able to use a virtual account if you are using WMF 5.0 on Windows 7 or Windows Server 2008 R2
$JEAConfigParams = @{
    SessionType = 'RestrictedRemoteServer'
    RunAsVirtualAccount = $true
    RoleDefinitions = @{
        $NonAdministrator = @{ RoleCapabilities = 'Maintenance' }
    }
    TranscriptDirectory = "$env:ProgramData\JEAConfiguration\Transcripts"
}

# Set up a folder for the Session Configuration files
if (-not (Test-Path "$env:ProgramData\JEAConfiguration"))
{
    New-Item -Path "$env:ProgramData\JEAConfiguration" -ItemType Directory
}

# Specify the name of the JEA endpoint
$sessionName = 'JEA_Demo'

if (Get-PSSessionConfiguration -Name $sessionName -ErrorAction SilentlyContinue)
{
    Unregister-PSSessionConfiguration -Name $sessionName -ErrorAction Stop
}

New-PSSessionConfigurationFile -Path "$env:ProgramData\JEAConfiguration\JEADemo.pssc" @JEAConfigParams

# Register the session configuration
Register-PSSessionConfiguration -Name $sessionName -Path "$env:ProgramData\JEAConfiguration\JEADemo.pssc"
```

## Aktivieren der PowerShell-Modulprotokollierung (optional)
Die folgenden Schritte aktivieren die Protokollierung für alle PowerShell-Aktionen in Ihrem System.
Sie müssen die Protokollierung nicht aktivieren, damit JEA funktioniert, sie ist jedoch sehr nützlich für den Abschnitt [Berichterstellung in JEA](reporting-on-jea.md).

1. Öffnen Sie den Editor für lokale Gruppenrichtlinien.
2. Navigieren Sie zu „Computerkonfiguration\Administrative Vorlagen\Windows-Komponenten\Windows-PowerShell“.
3. Doppelklicken Sie auf „Modulprotokollierung aktivieren“.
4. Klicken Sie auf „Aktiviert“.
5. Klicken Sie im Abschnitt „Optionen“ neben den Modulnamen auf „Anzeigen“.
6. Typ "\ *" in der Popup-Fenster. Dies bedeutet, dass PowerShell Befehle aus allen Modulen protokolliert.
7. Klicken Sie auf „OK“, und wenden Sie die Richtlinie an.

Hinweis: Sie können über eine Gruppenrichtlinie auch die systemweite PowerShell-Aufzeichnung aktivieren.

**Herzlichen Glückwunsch, Sie haben jetzt Ihren Computer mit der Demo-Endpunkt konfiguriert und erste Schritte mit JEA!**




<!--HONumber=Oct16_HO1-->


