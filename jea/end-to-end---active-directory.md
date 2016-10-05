---
description: 
manager: dongill
ms.topic: article
author: jpjofre
ms.prod: powershell
keywords: powershell,cmdlet,jea
ms.date: 2016-06-22
title: End-to-End Active Directory
ms.technology: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: e7ea3957ce3bbd3ce0fc072a82cd108606f05614

---

# End-to-End – Active Directory
Angenommen, der Umfang Ihres Programms hat sich erhöht.
Sie sollen jetzt JEA zu Domänencontrollern hinzufügen, damit Active Directory-Aktionen ausgeführt werden können.
Die Helpdeskmitarbeiter werden JEA verwenden, um Konten zu entsperren, Kennwörter zurückzusetzen und ähnliche Aktionen auszuführen.

Sie müssen einer anderen Gruppe von Benutzern einen vollkommen neuen Satz an Befehlen verfügbar machen.
Außerdem müssen Sie eine Vielzahl von vorhandenen Active Directory-Skripts verfügbar machen.
Dieser Abschnitt leitet Sie durch die Erstellung einer Sitzungskonfiguration sowie einer Rollenfunktion für diese Aufgabe.

## Voraussetzungen
Um diesen Abschnitt schrittweise durchzugehen, müssen Sie auf einem Domänencontroller arbeiten.
Machen Sie sich keine Sorgen, wenn Sie keinen Zugriff auf Ihren Domänencontroller haben.
Folgen Sie den Anweisungen einfach mit einem anderen Szenario oder einer anderen Rolle, mit der Sie vertraut sind.
Wenn Sie schnell einen neuen Domänencontroller einrichten möchten, finden Sie die notwendigen Informationen im Anhang unter [Erstellen eines Domänencontrollers](.\creating-a-domain-controller.md).

## Schritte zum Erstellen einer neuen Rollenfunktion und Sitzungskonfiguration

Das Erstellen einer neuen Rollenfunktion kann auf den ersten Blick eine große Herausforderung sein. Die Aufgabe lässt sich jedoch in relativ einfache Schritte unterteilen:

1.  Ermitteln Sie die Aufgaben, die Sie aktivieren müssen.
2.  Schränken Sie diese Aufgaben nach Bedarf ein.
3.  Stellen Sie sicher, dass die Aufgaben mit JEA funktionieren.
4.  Speichern Sie sie in einer Rollenfunktionsdatei.
5.  Registrieren Sie eine Sitzungskonfiguration, die diese Rollenfunktion verfügbar macht.

## Schritt 1: Ermitteln Sie die Aktionen und Aufgaben, die verfügbar gemacht werden müssen.
Bevor Sie eine neue Rollenfunktion oder Sitzungskonfiguration erstellen können, müssen Sie alle Aktionen und Aufgaben identifizieren, die Benutzer über den JEA-Endpunkt ausführen können müssen. Sie müssen ebenfalls ermitteln, wie diese Aktionen und Aufgaben in PowerShell ausgeführt werden.
Dieser Schritt erfordert sehr viel Recherche und die Erfassung von Anforderungen.
Die Art und Weise, wie Sie diesen Prozess durchführen, richtet sich nach Ihrer Organisation und Ihren Zielen.
Die Erfassung von Anforderungen und die Recherche sind in der Praxis ein entscheidender Bestandteil des Prozesses.
Dies ist möglicherweise der schwierigste Schritt im gesamten Prozess der Einrichtung von JEA.

### Suchen nach Ressourcen
Folgende Onlineressourcen bieten nützliche Informationen zum Erstellen eines Active Directory-Verwaltungsendpunkts:
-   [Übersicht über Active Directory-PowerShell](http://blogs.msdn.com/b/adpowershell/archive/2009/03/05/active-directory-powershell-overview.aspx)
-   [CMD PowerShell-Handbuch für Active Directory](http://blogs.technet.com/b/ashleymcglone/archive/2013/01/02/free-download-cmd-to-powershell-guide-for-ad.aspx)

### Erstellen einer Liste
Mit den zehn folgenden Aktionen werden Sie im Rest dieses Abschnitts arbeiten.
Denken Sie daran, dass es sich hier nur um ein Beispiel handelt und Ihre Organisation höchstwahrscheinlich andere Anforderungen stellt:

|Aktion                                                         |PowerShell-Befehl                                             |
|---------------------------------------------------------------|---------------------------------------------------------------|
|Konto entsperren                                                 |`Unlock-ADAccount`                                             |
|Kennwort zurücksetzen                                                 |`Set-ADAccountPassword` und `Set-ADUser -ChangePasswordAtLogon`|
|Position eines Benutzers ändern                                          |`Set-ADUser -Title`                                            |  
|AD-Konten finden, die gesperrt oder deaktiviert wurden oder inaktiv sind usw. |`Search-ADAccount`                                             |
|Benutzer zu Gruppe hinzufügen                                              |`Add-ADGroupMember -Identity (with whitelist) -Members`        |
|Benutzer aus Gruppe entfernen                                         |`Remove-ADGroupMember -Identity (with whitelist) -Members`     |
|Benutzerkonto aktivieren                                          |`Enable-ADAccount`                                             |
|Benutzerkonto deaktivieren                                         |`Disable-ADAccount`                                            |

## Schritt 2: Schränken Sie die Aufgaben nach Bedarf ein.

Nachdem Sie jetzt über die Liste der notwendigen Aktionen verfügen, müssen Sie die Funktion jedes einzelnen Befehls gründlich durchdenken.
Dafür gibt es zwei wichtige Gründe:

1.  Es kann leicht passieren, dass Sie Benutzern mehr Funktionen bieten als gewollt.
`Set-ADUser` z. B. ist ein unglaublich leistungsfähiger und flexibler Befehl.
Sie möchten sicher nicht die gesamte Funktionalität dieses Befehls für Helpdeskbenutzer verfügbar machen.  

2.  Schlimmer noch: Es kann passieren, dass Sie Befehle verfügbar machen, die es Benutzern ermöglichen, die Beschränkungen von JEA zu umgehen.
In diesem Fall kann JEA nicht mehr als Sicherheitsmechanismus fungieren.
Gehen Sie daher beim Auswählen von Befehlen mit größter Umsicht vor.
Invoke-Expression ermöglicht Benutzern beispielsweise die Ausführung von uneingeschränktem Code.
Weitere Informationen zu diesem Thema finden Sie im Abschnitt „Überlegungen zum Einschränken von Befehlen“.

Nachdem Sie jeden Befehl sorgfältig geprüft haben, entscheiden Sie sich für folgende Einschränkungen:

1.  `Set-ADUser` nur zugelassen werden sollte, mit der – Title-parameter

2.  `Add-ADGroupMember` und `Remove-ADGroupMember` können nur auf bestimmte Gruppen

### Schritt 3: Bestätigen Sie, dass die Aufgaben mit JEA funktionieren.
Die Verwendung dieser Cmdlets ist in der eingeschränkten JEA-Umgebung möglicherweise nicht ganz einfach.
JEA wird im Modus *NoLanguage* ausgeführt, der unter anderem verhindert, dass Benutzer Variablen verwenden dürfen.
Um die Benutzerfreundlichkeit sicherzustellen, müssen Sie einige Dinge prüfen.

Sehen Sie sich z. B. `Set-ADAccountPassword` an.
Der Parameter „-NewPassword“ erfordert eine sichere Zeichenfolge.
Benutzer erstellen häufig eine sichere Zeichenfolge und übergeben sie als Variable, wie hier zu sehen:

```PowerShell
$newPassword = Read-Host -Prompt "Specify a new password" -AsSecureString
Set-ADAccountPassword -Identity mollyd -NewPassword $newPassword -Reset
```

Der Modus *NoLanguage* verhindert jedoch die Verwendung von Variablen.
Es gibt zwei Möglichkeiten, diese Einschränkung zu umgehen:

1.  Sie können festlegen, dass Benutzer den Befehl ohne Variablenzuweisung ausführen.
Dies lässt sich einfach konfigurieren, kann jedoch für die Operatoren, die den Endpunkt verwenden, äußerst mühsam sein.
Wer möchte denn beim Zurücksetzen eines Kennworts jedes Mal die sichere Zeichenfolge eingeben?
```PowerShell
Set-ADAccountPassword -Identity mollyd -NewPassword (Read-Host -Prompt "Specify a new password" -AsSecureString) -Reset
```

2.  Sie können die Komplexität in einem Skript oder einer Funktion verpacken, das bzw. die Sie für die Endbenutzer verfügbar machen.
Skripts und Funktionen, die Sie verfügbar machen, werden in einem uneingeschränkten Kontext ausgeführt. Sie können völlig problemlos Funktionen schreiben, die Variablen verwenden und andere Befehle aufrufen.
Diese Vorgehensweise verbessert die Benutzerfreundlichkeit, verhindert Fehler, erfordert weniger PowerShell-Kenntnisse und senkt das Risiko, dass versehentlich zu viel Funktionalität verfügbar gemacht wird.
Der einzige Nachteil ist der Aufwand für die Erstellung und Verwaltung der Funktion.

### Exkurs: Hinzufügen einer Funktion zu Ihrem Modul
Mithilfe von Vorgehensweise Nr. 2 werden Sie eine PowerShell-Funktion namens `Reset-ContosoUserPassword` schreiben.
Diese Funktion führt alle Aktionen aus, die erforderlich sind, wenn Sie das Kennwort eines Benutzers zurücksetzen.
In Ihrer Organisation gehören dazu möglicherweise hochkomplexe Prozesse.
Da es sich hier nur um ein Beispiel handelt, setzt Ihr Befehl einfach nur das Kennwort zurück und fordert den Benutzer bei der Anmeldung zur Änderung des Kennworts auf.
Wir speichern den Befehl in dem Modul „Contoso_AD_Module“, das Sie im letzten Abschnitt erstellt haben.

1. Öffnen Sie in der PowerShell ISE die Datei „Contoso_AD_Module.psm1“.
```PowerShell
ise 'C:\Program Files\WindowsPowerShell\Modules\Contoso_AD_Module\Contoso_AD_Module.psm1'
```

2. Drücken Sie STRG+J, um das Menü für Codeausschnitte zu öffnen.

3. Suchen Sie nach „function“, und drücken Sie die EINGABETASTE.
Dadurch erhalten Sie ein simples Grundgerüst einer Funktion.

4. Benennen Sie die Funktion in „Reset-ContosoUserPassword“ um.  

5. Benennen Sie den einen Parameter in „Identity“ um, und löschen Sie den zweiten.

6. Kopieren Sie Folgendes in den Hauptteil der Funktion.
```PowerShell
# Get the new password
$NewPassword = Read-Host -Prompt "Enter a new password" -AsSecureString
# Reset the password
Set-ADAccountPassword -Identity $Identity -NewPassword $NewPassword -Reset
# Require the user to reset at next logon
Set-ADUser -Identity $Identity -ChangePasswordAtLogon
```

7. Speichern Sie die Datei.
Das Resultat sollte in etwa folgendermaßen aussehen:
```PowerShell
function Reset-ContosoUserPassword ($Identity)
{
# Get the new password
$NewPassword = Read-Host -Prompt "Enter a new password" -AsSecureString
# Reset the password
Set-ADAccountPassword -Identity $Identity -NewPassword $NewPassword -Reset
# Require the user to reset at next logon
Set-ADUser -Identity $Identity -ChangePasswordAtLogon
}
```
Jetzt können Ihre Benutzer einfach `Reset-ContosoUserPassword` aufrufen und müssen sich nicht die Syntax merken, um inline eine sichere Zeichenfolge zu erstellen.

## Schritt 4: Bearbeiten Sie die Rollenfunktionsdatei.
Im Abschnitt [Erstellen von Rollenfunktionen](./role-capabilities.md#role-capability-creation) haben Sie eine leere Rollenfunktionsdatei erstellt.
In diesem Abschnitt werden Sie diese Datei mit Werten füllen.

Öffnen Sie zunächst die Rollenfunktionsdatei in der PowerShell ISE.
```PowerShell
ise 'C:\Program Files\WindowsPowerShell\Modules\Contoso_AD_Module\RoleCapabilities\ADHelpDesk.psrc'
```
Aktualisieren Sie die Datei mit folgenden Änderungen:
```PowerShell
# OLD: VisibleCmdlets = 'Invoke-Cmdlet1', @{ Name = 'Invoke-Cmdlet2'; Parameters = @{ Name = 'Parameter1'; ValidateSet = 'Item1', 'Item2' }, @{ Name = 'Parameter2'; ValidatePattern = 'L*' } }
VisibleCmdlets =
    'Unlock-ADAccount',
    'Search-ADAccount',
    'Enable-ADAccount',
    'Disable-ADAccount',
    @{ Name = 'Set-ADUser'; Parameters = @{ Name = 'Title'; ValidateSet = 'Manager', 'Engineer' }},
    @{ Name = 'Add-ADGroupMember'; Parameters =
        @{Name = 'Identity'; ValidateSet = 'TestGroup'},
        @{Name = 'Members'}},
    @{ Name = 'Remove-ADGroupMember'; Parameters =
        @{Name = 'Identity'; ValidateSet = 'TestGroup'},
        @{Name = 'Members'}}

# OLD: VisibleFunctions = 'Invoke-Function1', @{ Name = 'Invoke-Function2'; Parameters = @{ Name = 'Parameter1'; ValidateSet = 'Item1', 'Item2' }, @{ Name = 'Parameter2'; ValidatePattern = 'L*' } }
VisibleFunctions = 'Reset-ContosoUserPassword'
```

In obigem Beispiel gilt es einiges zu beachten:
1.  PowerShell versucht, die Module automatisch zu laden, die für Ihre Rollenfunktion benötigt werden.
Möglicherweise müssen Sie Modulnamen explizit im Feld „ModulesToImport“ auflisten, wenn Sie feststellen, dass ein Modul nicht automatisch geladen wird.

2.  Wenn Sie nicht sicher sind, ob es sich bei einem Befehl um ein Cmdlet oder eine Funktion handelt, führen Sie `Get-Command` aus, und sehen Sie sich den Wert der „CommandType“-Eigenschaft an.

3.  Das ValidatePattern-Element ermöglicht die Verwendung eines regulären Ausdrucks, um Parameterargumente einzuschränken, falls zulässige Werte nicht einfach zu definieren sind.
Sie können für einen einzigen Parameter nicht sowohl ValidatePattern als auch ValidateSet definieren.

## Schritt 5: Erstellen und registrieren Sie eine neue Sitzungskonfiguration.
Als Nächstes erstellen Sie eine neue Sitzungskonfigurationsdatei, die Ihre Rollenfunktion für Mitglieder der AD-Gruppe „JEA_NonAdmin_HelpDesk“ verfügbar macht.

Erstellen und öffnen Sie zunächst eine neue leere Sitzungskonfigurationsdatei in der PowerShell ISE.
```PowerShell
New-PSSessionConfigurationFile -Path "$env:ProgramData\JEAConfiguration\HelpDeskDemo.pssc"
ise "$env:ProgramData\JEAConfiguration\HelpDeskDemo.pssc"
```
Ändern Sie die folgenden Felder in der PSSC-Datei.
Wenn Sie in Ihrer eigenen Umgebung arbeiten, sollten Sie „CONTOSO\JEA_NonAdmins_Helpdesk“ durch Ihre eigene Gruppe ohne Administratorrechte ersetzen.
```PowerShell
# OLD: Description = ''
Description = 'An endpoint for Active Directory tasks.'

# OLD: SessionType = 'Default'
SessionType = 'RestrictedRemoteServer'

# OLD: TranscriptDirectory = 'C:\Transcripts\'
TranscriptDirectory = "C:\ProgramData\JEAConfiguration\Transcripts"

# OLD: RunAsVirtualAccount = $true
RunAsVirtualAccount = $true

# OLD: RoleDefinitions = @{ 'CONTOSO\SqlAdmins' = @{ RoleCapabilities = 'SqlAdministration' }; 'CONTOSO\ServerMonitors' = @{ VisibleCmdlets = 'Get-Process' } }
RoleDefinitions = @{ 'CONTOSO\JEA_NonAdmin_HelpDesk' = @{ RoleCapabilities =  'ADHelpDesk' }}
```
Speichern und registrieren Sie die Sitzungskonfiguration.
```PowerShell
Register-PSSessionConfiguration -Name ADHelpDesk -Path "$env:ProgramData\JEAConfiguration\HelpDeskDemo.pssc"
```
## Probieren Sie es aus!
Rufen Sie Ihre Benutzeranmeldeinformationen ohne Administratorrechte ab:
```PowerShell
$HelpDeskCred = Get-Credential
```
Wenn Sie den Abschnitt [Set Up Users and Groups](creating-a-domain-controller.md#set-up-users-and-groups) (Einrichten von Benutzern und Gruppen) absolviert haben, lauten die Informationen wie folgt:
-   Benutzername = HelpDeskUser
-   Kennwort = pa$$w0rd

Stellen Sie eine Remoteverbindung mit dem „ADHelpdesk“ -Endpunkt her, und verwenden Sie dabei diese Anmeldeinformationen ohne Administratorrechte:
```PowerShell
Enter-PSSession -ComputerName . -ConfigurationName ADHelpDesk -Credential $HelpDeskCred
```
Verwenden Sie Set-ADUser, um die Position eines Benutzers zurückzusetzen.
```PowerShell
Set-ADUser -Identity OperatorUser -Title Engineer
```
Überprüfen Sie, ob die Position geändert wurde.
```PowerShell
Get-ADUser -Identity OperatorUser -Property Title
```
Verwenden Sie jetzt Add-ADGroupMember, um einen Benutzer zur Gruppe TestGroup hinzuzufügen.
Hinweis: Stellen Sie sicher, dass Sie die Gruppe TestGroup zuvor erstellt haben.
```PowerShell
Add-ADGroupMember TestGroup -Member OperatorUser -Verbose
```
Beenden Sie die Sitzung:
```PowerShell
Exit-PSSession
```
## Wichtige Konzepte
**NoLanguage-Modus**: Wenn PowerShell sich im Modus „NoLanguage“ befindet, dürfen Benutzer nur Befehle ausführen und keine Sprachelemente verwenden.
Wenn Sie hierzu weitere Informationen benötigen, führen Sie `Get-Help about_Language_Modes` aus.

**PowerShell-Funktionen**: Bei PowerShell-Funktionen handelt es sich um PowerShell-Codeabschnitte, denen Sie einen Namen zuweisen können.
Wenn Sie hierzu weitere Informationen benötigen, führen Sie `Get-Help about_Functions` aus.

**ValidateSet/ValidatePattern**: Wenn Sie einen Befehl verfügbar machen, können Sie die gültigen Argumente für bestimmte Parameter einschränken.
Ein „ValidateSet“-Element ist eine spezifische Liste gültiger Argumente.
Ein ValidatePattern-Element ist ein regulärer Ausdruck, mit dem die Argumente für diesen Parameter übereinstimmen müssen.




<!--HONumber=Oct16_HO1-->


