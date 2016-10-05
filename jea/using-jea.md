---
description: 
manager: dongill
ms.topic: article
author: jpjofre
ms.prod: powershell
keywords: powershell,cmdlet,jea
ms.date: 2016-06-22
title: Verwenden von JEA
ms.technology: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 9db7a5a91d25d459313117da34af63016f03c241

---

# Verwenden von JEA
In diesem Abschnitt geht es um das Endbenutzererlebnis beim *Verwenden von JEA*.
Im Abschnitt „Voraussetzungen“ haben Sie einen JEA-Demoendpunkt erstellt.
Wir verwenden diesen Demoendpunkt jetzt, um JEA in Aktion zu zeigen.
In späteren Abschnitten arbeiten wir uns sozusagen rückwärts an das Thema heran und stellen die Aktionen und Dateien vor, die dieses Endbenutzererlebnis ermöglichen.

## Verwenden von JEA als Benutzer ohne Administratorrechte
Um JEA in Aktion zu erleben, müssen Sie PowerShell-Remoting so verwenden, als wären Sie ein Benutzer ohne Administratorrechte.
Führen Sie folgenden Befehl in einem neuen PowerShell-Fenster aus:   

```PowerShell
$NonAdminCred = Get-Credential
```

Wenn Sie dazu aufgefordert werden, geben Sie die Anmeldeinformationen für Ihr Benutzerkonto ohne Administratorrechte ein.
Wenn Sie die Schritte im Abschnitt [Einrichten von Benutzern und Gruppen](creating-a-domain-controller.md#set-up-users-and-groups) durchgeführt haben, lauten die Informationen wie folgt:
-   Benutzername = OperatorUser
-   Kennwort = pa$$w0rd

Führen Sie als Nächstes den folgenden Befehl aus, um unter Verwendung der bereitgestellten Anmeldeinformationen eine Verbindung mit dem Demoendpunkt herzustellen:

```PowerShell
Enter-PSSession -ComputerName . -ConfigurationName JEA_Demo -Credential $NonAdminCred
```

Sie haben jetzt eine interaktive PowerShell-Remotesitzung auf dem lokalen Computer gestartet.
Durch Verwendung des Credential-Parameters haben Sie die Verbindung so hergestellt, *als wären Sie* der Benutzer „OperatorUser“ (bzw. der Benutzer, dessen Kontoinformationen Sie verwendet haben).
Die Eingabeaufforderung ändert sich zu `[localhost]: PS>`, was darauf hinweist, dass Sie in einer Remotesitzung arbeiten.  

Führen Sie folgenden Befehl an der Remoteeingabeaufforderung aus, um die Befehle anzuzeigen, die Ihnen zur Verfügung stehen:

```PowerShell
Get-Command
```

Wie Sie sehen, steht Ihnen nur eine sehr eingeschränkte Teilmenge der Befehle eines normalen PowerShell-Fensters zur Verfügung (bei denen es sich durchaus um mehrere Tausend handeln kann).
Genau gesagt: Das Fenster zeigt nur die acht JEA-Standardbefehle (Clear-Host, Exit-PSSession, Get-Command, Get-FormatData, Get-Help, Measure-Object, Out-Default, Select-Object) sowie die beiden Befehle an, die explizit in der Rollenfunktionsdatei für die Wartungsrolle enthalten sind.

Sehen Sie sich jetzt den Benutzerkontext an, in dem diese Sitzung ausgeführt wird, indem Sie die benutzerdefinierte Funktion aufrufen, die in der Rollenfunktionsdatei für die Wartungsrolle enthalten ist.

```PowerShell
Get-UserInfo
```

Die Ausgabe dieser Funktion zeigt „ConnectedUser“ sowie „RunAsUser“.
Der verbundene Benutzer ist das Konto, über das die Verbindung mit der Remotesitzung hergestellt wurde (also z. B. Ihr Konto).
Der verbundene Benutzer benötigt keine Administratorrechte.
Das ausführende Konto ist das Konto, das tatsächlich die Aktionen ausführt, für die Berechtigungen erforderlich sind.
Durch Herstellen einer Verbindung als ein Benutzer und anschließendes Nutzen des Kontos eines anderen Benutzers, der über Administratorrechte verfügt, können Sie nicht privilegierten Benutzern erlauben, bestimmte Verwaltungsaufgaben auszuführen, ohne ihnen Administratorrechte zu erteilen.

Führen Sie den folgenden Befehl aus, um dieses Konzept in Aktion zu erleben:

```PowerShell
Restart-Service -Name Spooler -Verbose
```

Normalerweise sind für die Ausführung von Restart-Service Administratorrechte erforderlich.
Über das virtuelle JEA-Konto können Sie diesen Befehl jedoch auch mit den Anmeldeinformationen eines Benutzers ausführen, der diese Berechtigungen nicht besitzt.

Mit JEA können Sie Ihre Arbeit also mit den Befehlen erledigen, die Sie bereits verwenden.
Was aber ist mit Befehlen, deren Ausführung *nicht* erlaubt sein sollte?
Versuchen Sie, einen anderen Befehl in der JEA-Sitzung auszuführen – z. B. `Restart-Computer` –, und sehen Sie selbst, wie JEA die Ausführung solcher Befehle verhindert.

```PowerShell
[localhost]: PS> Restart-Computer
The term 'Restart-Computer' is not recognized as the name of a cmdlet, function, script file, or
operable program. Check the spelling of the name, or if a path was included, verify that the path
is correct and try again.
    + CategoryInfo          : ObjectNotFound: (Restart-Computer:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException
```

Um den eingeschränkten JEA-Endpunkt zu verlassen, führen Sie folgenden Befehl aus:

```PowerShell
Exit-PSSession
```

Dadurch wird die Verbindung mit der PowerShell-Remotesitzung getrennt.




<!--HONumber=Oct16_HO1-->


