---
title: Verbesserungen bei Just Enough Administration
contributor: ryanpu
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 7d2f293f000d3d82f4a227d3b3760988d9f02be7

---

## Eingeschränktes Kopieren von Dateien auf einen bzw. von einem JEA-Endpunkt

Sie können Dateien jetzt remote auf einen bzw. von einem JEA-Endpunkt kopieren und sich dabei darauf verlassen, dass der Benutzer, der die Verbindung herstellt, keine *beliebige* Datei auf Ihrem System kopieren kann.
Dies ist möglich, indem Sie Ihre PSSC-Datei für die Bereitstellung eines Benutzerlaufwerks konfigurieren, mit dem sich Benutzer verbinden können.
Das Benutzerlaufwerk ist ein neues PSDrive, das für jeden Benutzer, der eine Verbindung herstellt, einzigartig ist und über mehrere Sitzungen hinweg beibehalten wird.
Wenn zum Kopieren von Dateien in eine bzw. aus einer JEA-Sitzung „Copy-Item“ verwendet wird, wird der Zugriff nur auf das Benutzerlaufwerk gestattet.
Beim Versuch, Dateien auf ein anderes PSDrive zu kopieren, wird der Vorgang mit einem Fehler abgebrochen.

Verwenden Sie die folgenden neuen Felder, um das Benutzerlaufwerk in Ihrer Konfigurationsdatei für JEA-Sitzungen einzurichten:
```powershell
MountUserDrive = $true
UserDriveMaximumSize = 10485760    # 10 MB
```

Der Ordner, sichern das Laufwerk wird erstellt `$env:LOCALAPPDATA\Microsoft\Windows\PowerShell\DriveRoots\DOMAIN_USER`

Um das Benutzerlaufwerk zu verwenden und Dateien auf einen bzw. von einem JEA-Endpunkt zu kopieren, der für die Offenlegung des Benutzerlaufwerks konfiguriert ist, verwenden Sie „Copy-Item“ mit den Parametern `-ToSession` und `-FromSession`.
```powershell
# Connect to the JEA endpoint
$jeasession = New-PSSession -ComputerName 'srv01' -ConfigurationName 'UserDemo'

# Copy a file in the local folder to the remote machine.
# Note: you cannot specify the file name or subfolder on the remote machine. You must exactly type "User:"
Copy-Item -Path .\SampleFile.txt -Destination User: -ToSession $jeasession

# Copy the file back from the remote machine to your local machine
Copy-Item -Path User:\SampleFile.txt -Destination . -FromSession $jeasession
```

Anschließend können Sie benutzerdefinierte Funktionen schreiben, um die auf dem Benutzerlaufwerk gespeicherten Daten zu verarbeiten und für Benutzer in Ihrer Datei für Rollenfunktionen bereitzustellen.

## Unterstützung für gruppenverwaltete Dienstkonten

In einigen Fällen muss ein Task, den ein Benutzer in einer JEA-Sitzung ausführen muss, auf Ressourcen außerhalb des lokalen Computers zugreifen.
Wenn eine JEA-Sitzung für die Verwendung eines virtuellen Kontos konfiguriert ist, wird bei jedem Versuch, auf diese Ressourcen zuzugreifen, die Identität des lokalen Computers (nicht des virtuellen Kontos oder des verbundenen Benutzers) als Ursprung dieses Zugriffsversuchs angezeigt.
In TP5 haben wir Unterstützung für die Ausführung von JEA im Kontext einer [Group Managed Service Account] (https://technet.microsoft.com/en-us/library/jj128431 (v=ws.11\).aspx), daher einfacher Zugriff auf Netzwerkressourcen mithilfe einer DNS-Identität. aktiviert.

Um eine JEA-Sitzung für die Ausführung über ein gruppenverwaltetes Dienstkonto zu konfigurieren, verwenden Sie den folgenden neuen Schlüssel in Ihrer PSSC-Datei:
```powershell
# Provide the name of your gMSA account here (don't include a trailing $)
# The local machine must be privileged to use this gMSA in Active Directory
GroupManagedServiceAccount = 'myGMSAforJEA'

# You cannot configure a JEA endpoint to use both a gMSA and virtual account
# You can leave the RunAsVirtualAccount field commented out or explicitly set it to false
RunAsVirtualAccount = $false
```

> **Hinweis:** Gruppenverwaltete Dienstkonten bieten nicht die Isolation oder den begrenzten Bereich virtueller Konten.
> Jeder verbundene Benutzer verwendet dieselbe Identität des gruppenverwalteten Dienstkontos, das möglicherweise über Berechtigungen für Ihr gesamtes Unternehmen verfügt.
> Gehen Sie daher mit Bedacht vor, wenn Sie die Verwendung eines gruppenverwalteten Dienstkontos wählen. Wenn möglich, sollten Sie immer virtuelle Konten vorziehen, die auf den lokalen Computer beschränkt sind.

## Richtlinien für bedingten Zugriff

Bei JEA lässt sich hervorragend einschränken, welche Aufgaben ein Benutzer, der sich mit einem System verbunden hat, zum Verwalten des Systems ausführen darf. Doch wie gehen Sie vor, wenn Sie einschränken möchten, *wann* ein Benutzer JEA verwenden kann?
Wir haben die Sitzungskonfigurationsdateien (PSSC-Dateien) um Optionen erweitert, mit denen Sie Sicherheitsgruppen angeben können, deren Mitglied ein Benutzer sein muss, um eine JEA-Sitzung zu erstellen.
Dies kann insbesondere dann nützlich sein, wenn Sie in Ihrer Umgebung über ein JIT-System (Just In Time) verfügen und festlegen möchten, dass Ihre Benutzer über erhöhte Berechtigungen verfügen müssen, bevor sie auf einen JEA-Endpunkt mit hohen Berechtigungen zugreifen.

Über das neue Feld *RequiredGroups* in der PSSC-Datei können Sie die Logik angeben, mit der ermittelt wird, ob ein Benutzer sich mit JEA verbinden kann.
Dazu geben Sie eine (optional geschachtelte) Hashtabelle an, die zum Erstellen Ihrer Regeln die Schlüssel „And“ und „Or“ verwendet.
Nachfolgend finden Sie einige Beispiele für die Verwendung dieses Felds:
```powershell
# Example 1: Connecting users must belong to a security group called "elevated-jea"
RequiredGroups = @{ And = 'elevated-jea' }

# Example 2: Connecting users must have signed on with 2 factor authentication or a smart card
# The 2 factor authentication group name is "2FA-logon" and the smart card group name is "smartcard-logon"
RequiredGroups = @{ Or = '2FA-logon', 'smartcard-logon' }

# Example 3: Connecting users must elevate into "elevated-jea" with their JIT system and have logged on with 2FA or a smart card
RequiredGroups = @{ And = 'elevated-jea', @{ Or = '2FA-logon', 'smartcard-logon' }}
```

## Behoben: Virtuelle Konten werden jetzt auf Windows Server 2008 R2 unterstützt
In WMF 5.1 können nun virtuelle Konten auf Windows Server 2008 R2 verwendet werden, sodass für Windows Server 2008 R2 - 2016 jetzt konsistente Konfigurationen und übereinstimmende Features bereitgestellt werden.
Bei Verwendung von JEA unter Windows 7 werden virtuelle Konten weiterhin nicht unterstützt.


<!--HONumber=Oct16_HO1-->


