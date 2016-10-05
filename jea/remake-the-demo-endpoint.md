---
description: 
manager: dongill
ms.topic: article
author: jpjofre
ms.prod: powershell
keywords: powershell,cmdlet,jea
ms.date: 2016-06-22
title: Neuerstellen des Demoendpunkts
ms.technology: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: acd2cfbd038250a26236c875d0e8b03a32cd84f9

---

# Neuerstellen des Demoendpunkts
In diesem Abschnitt erfahren Sie, wie Sie ein exaktes Replikat des Demoendpunkts erstellen, das Sie im vorherigen Abschnitt verwendet haben.
Hierbei werden wichtige Konzepte vorgestellt, die für ein genaues Verständnis von JEA notwendig sind, einschließlich der PowerShell-Sitzungskonfigurationen und -Rollenfunktionen.

## PowerShell-Sitzungskonfigurationen
Bei der Verwendung von JEA im vorherigen Abschnitt bestand der erste Schritt darin, den folgenden Befehl auszuführen:

```PowerShell
Enter-PSSession -ComputerName . -ConfigurationName JEA_Demo -Credential $NonAdminCred
```

Die meisten Parameter sind selbsterklärend, der Parameter *ConfigurationName* kann jedoch auf den ersten Blick verwirrend sein.
Dieser Parameter gab die PowerShell-Sitzungskonfiguration an, mit der Sie eine Verbindung hergestellt haben.

*PowerShell-Sitzungskonfiguration* ist im Grunde nur eine andere, schickere Bezeichnung für einen PowerShell-Endpunkt.
Es ist im übertragenen Sinn der „Ort“, an dem Benutzer eine Verbindung herstellen und Zugriff auf PowerShell-Funktionen erhalten.
Je nachdem, wie Sie eine Sitzungskonfiguration einrichten, können Sie unterschiedliche Funktionen für die Benutzer bereitstellen, die eine Verbindung herstellen.
Bei JEA werden Sitzungskonfigurationen verwendet, um die in PowerShell verfügbaren Funktionen einzuschränken und eine Ausführung als privilegiertes virtuelles Konto zu ermöglichen.

Sie verfügen bereits über einige registrierte PowerShell-Sitzungskonfigurationen auf Ihrem Computer, die jeweils leicht unterschiedlich eingerichtet sind.
Die meisten Konfigurationen sind im Lieferumfang von Windows enthalten, die Sitzungskonfiguration „JEA_Demo“ wurde jedoch im Abschnitt [Voraussetzungen](prerequisites.md) eingerichtet.
Sie können alle registrierten Sitzungskonfigurationen anzeigen, indem Sie folgenden Befehl an einer PowerShell-Administratoreingabeaufforderung ausführen:

```PowerShell
Get-PSSessionConfiguration
```

## PowerShell-Sitzungskonfigurationsdateien
Sie können neuen Sitzungskonfigurationen erstellen, indem Sie neue *PowerShell-Sitzungskonfigurationsdateien* registrieren.
Sitzungskonfigurationsdateien weisen die Dateierweiterung PSSC auf.
Sie können Sitzungskonfigurationsdateien mit dem Cmdlet New-PSSessionConfigurationFile generieren.

Als Nächstes werden Sie eine neue Sitzungskonfiguration für JEA erstellen und registrieren.

## Generieren und Ändern der PowerShell-Sitzungskonfiguration
Führen Sie folgenden Befehl aus, um eine Gerüstdatei („Skeleton“) für eine PowerShell-Sitzungskonfiguration zu generieren.

```PowerShell
New-PSSessionConfigurationFile -Path "$env:ProgramData\JEAConfiguration\JEADemo2.pssc"
```

> **TIPP**: Standardmäßig sind nur die häufigsten Konfigurationseinstellungen in der Gerüstdatei vorhanden.
> Verwenden Sie den `-Full`-Parameter, um alle anwendbaren Einstellungen in die generierte PSSC-Datei einzuschließen.

Öffnen Sie die Datei in der PowerShell ISE oder einem Text-Editor Ihrer Wahl.

```PowerShell
ise "$env:ProgramData\JEAConfiguration\JEADemo2.pssc"
```

Aktualisieren Sie die folgenden Felder mit den nachstehenden Werten (denken Sie daran, Ihre eigene Sicherheitsgruppe ohne Administratorrechte einzusetzen):

```PowerShell
# OLD: SessionType = 'Default'
SessionType = 'RestrictedRemoteServer'

# OLD: TranscriptDirectory = 'C:\Transcripts\'
TranscriptDirectory = "C:\ProgramData\JEAConfiguration\Transcripts"

# OLD: # RunAsVirtualAccount = $true
RunAsVirtualAccount = $true

# OLD: RoleDefinitions = @{ 'CONTOSO\SqlAdmins' = @{ RoleCapabilities = 'SqlAdministration' }; 'CONTOSO\ServerMonitors' = @{ VisibleCmdlets = 'Get-Process' } }
RoleDefinitions = @{'CONTOSO\JEA_NonAdmin_Operator' = @{ RoleCapabilities =  'Maintenance' }}
```

Die Einträge haben folgende Bedeutung:

1.  Das Feld *SessionType* definiert vorab eingerichtete Standardeinstellungen, die für diesen Endpunkt verwendet werden sollen.
*RestrictedRemoteServer* definiert die Mindesteinstellungen, die für die Remoteverwaltung erforderlich sind.
Standardmäßig macht ein *RestrictedRemoteServer*-Endpunkt die Befehle Get-Command, Get-FormatData, Select-Object, Get-Help, Measure-Object, Exit-PSSession, Clea-Host, und Out-Default verfügbar.
Die Einstellung *ExecutionPolicy* wird auf *RemoteSigned* und die Einstellung *LanguageMode* auf *NoLanguage* festgelegt.
Das Ergebnis dieser Einstellungen ist ein sicherer und einfacher Startpunkt zum Konfigurieren des Endpunkts.

2.  Das Feld *RoleDefinitions* weist bestimmten Gruppen Rollenfunktionen zu.
Es definiert, welcher Benutzer mit einem privilegierten Konto welche Aufgaben ausführen darf.
Mit diesem Feld können Sie basierend auf der Gruppenmitgliedschaft die Funktionen angeben, die für einen verbundenen Benutzer zur Verfügung stehen.
Dies ist das Kernstück der RBAC-Funktionalität von JEA.
In diesem Beispiel machen Sie die vorab erstellte Rollenfunktion „Maintenance“ für Mitglieder der Gruppe „Contoso\JEA_NonAdmin_Operator“ verfügbar.

3.  Das Feld *RunAsVirtualAccount* gibt an, dass PowerShell auf diesem Endpunkt mit einem ausführenden virtuellen Konto ausgeführt werden soll.
Standardmäßig ist das virtuelle Konto Mitglied der integrierten Administratorengruppe.
Auf einem Domänencontroller ist es standardmäßig auch Mitglied der Domänenadministratorengruppe.
Im weiteren Verlauf dieses Leitfadens erfahren Sie, wie Sie die Berechtigungen des virtuellen Kontos anpassen.

4.  Das Feld *TranscriptDirectory* definiert, wo mitgeschnittene PowerShell-Aufzeichnungen nach jeder Remotesitzung gespeichert werden.
Diese Aufzeichnungen bieten eine gut lesbare Möglichkeit, die Aktionen zu überprüfen, die in jeder Sitzung ausgeführt wurden.
Weitere Informationen über PowerShell-Aufzeichnungen finden Sie in diesem [Blogbeitrag](http://blogs.msdn.com/b/powershell/archive/2015/06/09/powershell-the-blue-team.aspx).
Hinweis: Die reguläre Windows-Ereignisverwaltung erfasst ebenfalls Informationen darüber, welche Befehle von den einzelnen Benutzern mit PowerShell ausgeführt wurden.
PowerShell-Aufzeichnungen sind allerdings besser lesbar.

Speichern Sie Ihre Änderungen abschließend in *JEADemo2.pssc*.

## Anwenden der PowerShell-Sitzungskonfiguration

Um aus einer Sitzungskonfigurationsdatei einen Endpunkt zu erstellen, müssen Sie die Datei registrieren.
Dafür sind einige Informationen erforderlich:

1. Der Pfad zur Sitzungskonfigurationsdatei.
2. Der Name Ihrer registrierten Sitzungskonfiguration. Dies ist der gleiche Name, den Benutzer im ConfigurationName-Parameter angeben, wenn sie mithilfe von `Enter-PSSession` oder `New-PSSession` eine Verbindung mit Ihrem Endpunkt herstellen.

Um die Sitzungskonfiguration auf Ihrem lokalen Computer zu registrieren, führen Sie folgenden Befehl aus:

```PowerShell
Register-PSSessionConfiguration -Name 'JEADemo2' -Path "$env:ProgramData\JEAConfiguration\JEADemo2.pssc"
```

Herzlichen Glückwunsch! Sie haben Ihren JEA-Endpunkt eingerichtet.

## Testen Ihres Endpunkts
Führen Sie die Schritte im Abschnitt [Verwenden von JEA](using-jea.md) erneut für Ihren neuen Endpunkt aus, um zu überprüfen, ob er erwartungsgemäß ausgeführt wird.
Stellen Sie sicher, dass Sie einen neuen Endpunktnamen verwenden (JEADemo2), wenn Sie den Konfigurationsnamen in `Enter-PSSession` bereitstellen.

```PowerShell
Enter-PSSession -ComputerName . -ConfigurationName JEADemo2 -Credential $NonAdminCred
```

## Wichtige Konzepte
**PowerShell Session-Sitzungskonfiguration**: Wird auch als *PowerShell-Endpunkt* bezeichnet und ist im übertragenen Sinn der „Ort“, an dem Benutzer eine Verbindung herstellen und Zugriff auf PowerShell-Funktionen erhalten.
Sie können die in Ihrem System registrierten Sitzungskonfigurationen durch Ausführen von `Get-PSSessionConfiguration` auflisten.
Wenn eine PowerShell-Sitzungskonfiguration auf bestimmte Weise konfiguriert ist, kann sie auch als *JEA-Endpunkt* bezeichnet werden.

**PowerShell-Sitzungskonfigurationsdatei (PSSC)**: Eine Datei, die nach der Registrierung Einstellungen für eine PowerShell-Sitzungskonfiguration definiert.
Sie enthält Spezifikationen für Benutzerrollen, die eine Verbindung mit dem Endpunkt herstellen können, das ausführende virtuelle Konto sowie weitere Details.     

**RoleDefinitions**: Das Feld in einer Sitzungskonfigurationsdatei, das die Rollenfunktionen definiert, die verbundene Benutzer ausführen können.
Es definiert, *welcher Benutzer* mit einem privilegierten Konto *welche Aufgaben* ausführen darf.
Dies ist das Kernstück der RBAC-Funktionalität von JEA.

**SessionType**: Ein Feld in einer Sitzungskonfigurationsdatei, das Standardeinstellungen für eine Sitzungskonfiguration darstellt.
Bei JEA-Endpunkten muss dieses Feld auf RestrictedRemoteServer festgelegt sein.

**PowerShell-Aufzeichnung**: Eine Datei mit einem Mitschnitt einer PowerShell-Sitzung.
Sie können PowerShell mithilfe des Felds TranscriptDirectory so einrichten, dass Aufzeichnungen von JEA-Sitzungen generiert werden.
Weitere Informationen über Aufzeichnungen finden Sie in diesem [Blogbeitrag](https://technet.microsoft.com/en-us/magazine/ff687007.aspx).




<!--HONumber=Oct16_HO1-->


