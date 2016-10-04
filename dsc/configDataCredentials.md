---
title: "Optionen für Anmeldeinformationen in den Konfigurationsdaten"
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: a750fb208e73ce2ebffb2fa86a55c825169d8ad8

---

# Optionen für Anmeldeinformationen in den Konfigurationsdaten
>Gilt für: Windows PowerShell 5.0

## Nur-Text-Kennwörter und Domänenbenutzer

DSC-Konfigurationen mit unverschlüsselten Anmeldeinformationen generieren eine Fehlermeldung zu Nur-Text-Kennwörtern.
DSC generiert außerdem bei Verwendung von Domänenanmeldeinformationen eine Warnung.
Um diese Fehler- und Warnmeldungen zu unterdrücken, verwenden Sie die DSC-Konfigurationsdaten-Schlüsselwörter.
* **PsDscAllowPlainTextPassword**
* **PsDscAllowDomainUser**

## Behandeln von Anmeldeinformationen in DSC

DSC-Konfigurationsressourcen werden standardmäßig als `Local System` ausgeführt.
Einige Ressourcen benötigen jedoch Anmeldeinformationen, z. B. wenn die `Package`-Ressource Software unter einem bestimmten Benutzerkonto installieren muss.

Frühere Ressourcen verwendeten in diesem Fall einen hartcodierten `Credential`-Eigenschaftennamen.
In WMF 5.0 wurde eine automatische `PsDscRunAsCredential`-Eigenschaft für alle Ressourcen hinzugefügt. Weitere Informationen zur Verwendung von `PsDscRunAsCredential`, finden Sie unter [Ausführen von DSC mit Benutzeranmeldeinformationen](runAsUser.md).
Neuere und benutzerdefinierte Ressourcen können diese automatische Eigenschaft verwenden, anstatt eine eigene Eigenschaften für Anmeldeinformationen zu erstellen.

*Beachten Sie, dass das Design von Ressourcen mehrere Anmeldeinformationen für einen bestimmten Grund verwenden, und haben Sie ihre eigenen Eigenschaften für Anmeldeinformationen.*

Um die verfügbaren Eigenschaften für Anmeldeinformationen für eine Ressource zu finden, verwenden Sie entweder `Get-DscResource -Name ResourceName -Syntax` oder Intellisense in der ISE (`CTRL+SPACE`).

```PowerShell
PS C:\> Get-DscResource -Name Group -Syntax
Group [String] #ResourceName
{
    GroupName = [string]
    [Credential = [PSCredential]]
    [DependsOn = [string[]]]
    [Description = [string]]
    [Ensure = [string]{ Absent | Present }]
    [Members = [string[]]]
    [MembersToExclude = [string[]]]
    [MembersToInclude = [string[]]]
    [PsDscRunAsCredential = [PSCredential]]
}
```

In diesem Beispiel wird eine [Group](https://msdn.microsoft.com/en-us/powershell/dsc/groupresource)-Ressource aus dem integrierten DSC-Ressourcenmodul `PSDesiredStateConfiguration` verwendet.
Damit können lokale Gruppen erstellt oder Member hinzugefügt bzw. entfernt werden.
Es wird sowohl die Eigenschaft `Credential` als auch die automatische Eigenschaft `PsDscRunAsCredential` akzeptiert.
Allerdings verwendet die Ressource nur die Eigenschaft `Credential`.
Erfahren Sie mehr über `PsDscRunAsCredential` in den [WMF-Versionshinweise](https://msdn.microsoft.com/en-us/powershell/wmf/dsc_runas).

## Beispiel: Die Eigenschaft „Credential“ der Ressource „Group“

Der DSC-Dienst wird unter `Local System` ausgeführt, sodass er bereits über Berechtigungen zum Ändern lokaler Benutzer und Gruppen verfügt.
Wenn es sich bei dem hinzugefügten Member um ein lokales Konto handelt, sind keine Anmeldeinformationen erforderlich.
Wenn die Ressource `Group` ein Domänenkonto zur lokalen Gruppe hinzufügt, sind Anmeldeinformationen erforderlich.

Anonyme Abfragen an Active Directory sind nicht zulässig.
Die Eigenschaft `Credential` der Ressource `Group`ist das zum Abfragen von Active Directory verwendete Domänenkonto.
In den meisten Fällen könnte es sich dabei um ein allgemeines Benutzerkonto handeln, da Benutzer standardmäßig über *Lesezugriff* auf die meisten Objekte in Active Directory verfügen.

## Beispielkonfiguration

Der folgende Beispielcode verwendet DSC zum Auffüllen einer lokalen Gruppen mit einem Domänenbenutzer:

```PowerShell
Configuration DomainCredentialExample
{
    param
    (
        [PSCredential] $DomainCredential
    )
    Import-DscResource -ModuleName PSDesiredStateConfiguration

    node localhost
    {
        Group DomainUserToLocalGroup
        {
            GroupName        = 'ApplicationAdmins'
            MembersToInclude = 'contoso\alice'
            Credential       = $DomainCredential
        }
    }
}

$cred = Get-Credential -UserName contoso\genericuser -Message "Password please"
DomainCredentialExample -DomainCredential $cred
```

Dieser Code generiert eine Fehler- und eine Warnmeldung:

```
ConvertTo-MOFInstance : System.InvalidOperationException error processing
property 'Credential' OF TYPE 'Group': Converting and storing encrypted
passwords as plain text is not recommended. For more information on securing
credentials in MOF file, please refer to MSDN blog:
http://go.microsoft.com/fwlink/?LinkId=393729

At line:11 char:9
+   Group
At line:297 char:16
+     $aliasId = ConvertTo-MOFInstance $keywordName $canonicalizedValue
+                ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (:) [Write-Error], InvalidOperationException
    + FullyQualifiedErrorId : FailToProcessProperty,ConvertTo-MOFInstance

WARNING: It is not recommended to use domain credential for node 'localhost'.
In order to suppress the warning, you can add a property named
'PSDscAllowDomainUser' with a value of $true to your DSC configuration data
for node 'localhost'.
```

In diesem Beispiel gibt es zwei Probleme:
1.  Ein Fehler zeigt an, dass Nur-Text-Kennwörter nicht empfohlen werden.
2.  Eine Warnung rät davon ab, Domänenanmeldeinformationen zu verwenden.

## PsDscAllowPlainTextPassword

Die erste Fehlermeldung enthält eine URL zur Dokumentation.
Unter diesem Link wird erläutert, wie Kennwörtern mit einer [ConfigurationData](https://msdn.microsoft.com/en-us/powershell/dsc/configdata)-Struktur und einem Zertifikat verschlüsselt werden.
Weitere Informationen zu Zertifikaten und DSC [erhalten Sie in diesen Beitrag](http://aka.ms/certs4dsc).

Um ein Nur-Text-Kennwort zu erzwingen, muss im Konfigurationsdatenabschnitt der Ressource das Schlüsselwort `PsDscAllowPlainTextPassword` wie folgt enthalten sein:

```PowerShell
Configuration DomainCredentialExample
{
    param
    (
        [PSCredential] $DomainCredential
    )
    Import-DscResource -ModuleName PSDesiredStateConfiguration

    node localhost
    {
        Group DomainUserToLocalGroup
        {
            GroupName        = 'ApplicationAdmins'
            MembersToInclude = 'contoso\alice'
            Credential       = $DomainCredential
        }
    }
}

$cd = @{
    AllNodes = @(
        @{
            NodeName = 'localhost'
            PSDscAllowPlainTextPassword = $true
        }
    )
}

$cred = Get-Credential -UserName contoso\genericuser -Message "Password please"
DomainCredentialExample -DomainCredential $cred -ConfigurationData $cd
```

*Beachten Sie, dass für `NodeName` kein Sternchen angegeben werden darf, sondern der Name eines bestimmten Knotens obligatorisch ist.*

**Microsoft empfiehlt, um nur-Text-Kennwörter aufgrund der hohes Sicherheitsrisiko zu vermeiden.**

## Domänenanmeldeinformationen

Wenn Sie das Beispielkonfigurationsskript erneut ausführen (mit oder ohne Verschlüsselung), wird weiterhin die Warnung generiert, dass die Verwendung von Anmeldeinformationen eines Domänenkontos nicht empfohlen wird.
Durch Verwendung eines lokalen Kontos wird verhindert, dass Domänenanmeldeinformationen, die auf anderen Servern verwendet werden könnten, offengelegt werden.

**Wenn Sie Anmeldeinformationen mit DSC-Ressourcen zu verwenden, bevorzugen Sie ein lokales Konto über ein Domänenkonto ein, wenn möglich.**

Ist eine ' \' oder '@' in der `Username` Eigenschaft der Anmeldeinformationen dann das DSC behandelt es als ein Domänenkonto.
Ausnahmen machen „Localhost“, „127.0.0.1“ und „:: 1“ im Domänenteil des Benutzernamens.

## PSDscAllowDomainUser

Im oben stehenden Beispiel zur DSC-Ressourcen `Group` ist zum Abfragen einer Active Directory-Domäne ein Domänenkonto *erforderlich*.
Fügen Sie in diesem Fall die Eigenschaft `PSDscAllowDomainUser` wie folgt zum Block `ConfigurationData` hinzu:

```PowerShell
$cd = @{
    AllNodes = @(
        @{
            NodeName = 'localhost'
            PSDscAllowDomainUser = $true
            # PSDscAllowPlainTextPassword = $true
            CertificateFile = "C:\PublicKeys\server1.cer"
        }
    )
}
```

Nun generiert das Konfigurationsskript die MOF-Datei ohne Fehler oder Warnungen.




<!--HONumber=Oct16_HO1-->


