---
title: WinRMSecurityRedirect
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
redirect_url: https://msdn.microsoft.com/powershell/scripting/setup/winrmsecurity
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 207792452c563ec6cca5c17fbcd122372442d8ac

---

# Sicherheitsaspekte von PowerShell-Remoting

PowerShell-Remoting ist die empfohlene Methode zum Verwalten von Windows-Systemen. PowerShell-Remoting ist unter Windows Server 2012 R2 standardmäßig aktiviert. Dieses Dokument behandelt Sicherheitsaspekte, Empfehlungen und bewährte Methoden bei der Verwendung von PowerShell-Remoting.

## Was ist PowerShell-Remoting?

PowerShell-Remoting verwendet [Windows-Remoteverwaltung (WinRM)](https://msdn.microsoft.com/en-us/library/windows/desktop/aa384426.aspx), wobei es sich um die Microsoft-Implementierung des Protokolls von [Web Services for Management (WS-Management)](http://www.dmtf.org/sites/default/files/standards/documents/DSP0226_1.2.0.pdf) handelt, um Benutzern die Ausführung von PowerShell-Befehlen auf Remotecomputern zu ermöglichen. Weitere Informationen zur Verwendung von PowerShell-Remoting finden Sie unter [Ausführen von Remotebefehlen](https://technet.microsoft.com/en-us/library/dd819505.aspx).

PowerShell-Remoting unterscheidet sich von Ausführung eines Cmdlets auf einem Remotecomputer unter Verwendung des Parameters **ComputerName**, wobei ein Remoteprozeduraufruf (RPC) als zugrunde liegendes Protokoll verwendet wird.

##  Standardeinstellungen für PowerShell-Remoting

PowerShell-Remoting (und WinRM) lauschen an folgenden Ports:

- HTTP: 5985
- HTTPS: 5986

Standardmäßig gestattet PowerShell-Remoting nur Verbindungen von Mitgliedern der Gruppe „Administratoren“. Sitzungen werden im Kontext des Benutzers gestartet, weshalb alle Betriebssystem-Zugriffssteuerungen, die auf einzelne Benutzer und Gruppen angewendet wurden, für diese weiterhin gelten, während die Verbindung über PowerShell-Remoting besteht.

In privaten Netzwerken akzeptiert die Windows-Firewall-Standardregel für PowerShell-Remoting alle Verbindungen. In öffentlichen Netzwerken lässt die Windows-Firewall-Standardregel Verbindungen mit PowerShell-Remoting nur aus demselben Subnetz heraus zu. Sie müssen diese Regel explizit ändern, um PowerShell-Remoting für alle Verbindungen in einem öffentlichen Netzwerk zu öffnen.

>**Warnung:** Die Firewallregel für öffentliche Netzwerke soll den Computer vor potenziell schädlichen, externen Verbindungsversuchen schützen. Seien Sie vorsichtig beim Entfernen dieser Regel.

## Prozessisolation

PowerShell-Remoting verwendet [Windows-Remoteverwaltung (WinRM)](https://msdn.microsoft.com/en-us/library/windows/desktop/aa384426) für die Kommunikation zwischen Computern. WinRM wird als Dienst unter dem „Netzwerkdienst“-Konto ausgeführt, und erstellt isolierte Prozesse, die als Benutzerkonten ausgeführt werden, um PowerShell-Instanzen zu hosten. Eine Instanz von PowerShell, die als ein Benutzer ausgeführt wird, hat keinen Zugriff auf einen Prozess, der eine Instanz von PowerShell als ein anderer Benutzer ausführt.

## Von PowerShell-Remoting generierte Ereignisprotokolle

FireEye stellt eine gute Zusammenfassung der Ereignisprotokolle und andere Sicherheitsinformationen, die von PowerShell-Remotingsitzungen generiert werden, bereit unter  
[Untersuchung von PowerShell-Angriffen](https://www.fireeye.com/content/dam/fireeye-www/global/en/solutions/pdfs/wp-lazanciyan-investigating-powershell-attacks.pdf).

## Verschlüsselung und Transportprotokolle

Es ist hilfreich, die Sicherheit einer PowerShell-Remotingverbindung aus zwei Blickwinkeln zu betrachten: anfängliche Authentifizierung und laufende Kommunikation. 

Unabhängig vom verwendeten Transportprotokoll (HTTP oder HTTPS) verschlüsselt PowerShell-Remoting immer die gesamte Kommunikation im Anschluss an die anfängliche Authentifizierung mit einem sitzungsbezogenen, symmetrischen AES-256-Schlüssel.
    
### Anfängliche Authentifizierung

Mit der Authentifizierung wird die Identität des Clients gegenüber dem Server bestätigt und idealerweise die des Servers gegenüber dem Client.
    
Wenn ein Client eine Verbindung mit einem Domänenserver mithilfe seines Computernamens herstellt (d. h.: „server01“ oder „server01.contoso.com“), ist das Standardprotokoll für die Authentifizierung [Kerberos](https://msdn.microsoft.com/en-us/library/windows/desktop/aa378747.aspx).
Kerberos garantiert sowohl die Identität des Benutzers als auch die des Servers, ohne dabei jegliche Art von wiederverwendbaren Anmeldeinformationen zu senden.

Wenn ein Client eine Verbindung mit einem Domänenserver mithilfe seiner IP-Adresse oder eine Verbindung mit einem Arbeitsgruppenserver herstellt, ist eine Authentifizierung mittels Kerberos nicht möglich. In diesem Fall verwendet PowerShell-Remoting dann das [NTLM-Authentifizierungsprotokoll](https://msdn.microsoft.com/en-us/library/windows/desktop/aa378749.aspx). Die NTLM-Authentifizierung garantiert die Identität des Benutzers, ohne dabei jegliche Art von delegierbaren Anmeldeinformationen zu senden. Um die Identität des Benutzers nachzuweisen, erfordert das NTLM-Protokoll, dass sowohl der Client als auch der Server einen Sitzungsschlüssel aus dem Kennwort des Benutzers berechnen, ohne dabei jemals das Kennwort selbst auszutauschen. Der Server kennt in der Regel das Kennwort des Benutzers nicht, weshalb er mit dem Domänencontroller kommuniziert, der das Kennwort des Benutzers kennt und den Sitzungsschlüssel für den Server berechnet. 
      
Das NTLM-Protokoll garantiert jedoch nicht die Identität des Servers. Wie bei allen Protokollen, die NTLM für die Authentifizierung verwenden, könnte ein Angreifer, der Zugriff auf das Computerkonto eines der Domäne beigetretenen Computers hat, den Domänencontroller zur Berechnung eines NTLM-Sitzungsschlüssels aufrufen und auf diese Art die Identität des Servers annehmen.

NTLM-basierte Authentifizierung ist standardmäßig deaktiviert, kann aber zugelassen werden, indem entweder SSL auf dem Zielserver oder die WinRM-Einstellung „TrustedHosts“ konfiguriert wird.
    
#### Verwenden von SSL-Zertifikaten zum Überprüfen der Identität des Servers während NTLM-basierter Verbindungen

Da das NTLM-Authentifizierungsprotokoll nicht die Identität des Zielservers sicherstellen kann (nur, dass er bereits Ihr Kennwort kennt), können Sie Zielserver für die Verwendung von SSL für PowerShell-Remoting konfigurieren. Das Zuweisen eines SSL-Zertifikats zum Zielserver (sofern von einer Zertifizierungsstelle ausgestellt, der auch der Client vertraut) ermöglicht NTLM-basierte Authentifizierung, die sowohl die Identität des Benutzers als auch die des Servers garantiert.
    
#### Ignorieren von NTLM-basierten Serveridentitätsfehlern
      
Wenn es nicht möglich ist, ein SSL-Zertifikat auf einem Server für NTLM-Verbindungen bereitzustellen, können Sie die daraus resultierenden Identitätsfehler unterdrücken, indem Sie den Server zur WinRM-Liste **TrustedHosts** hinzufügen. Beachten Sie, dass das Hinzufügen eines Servernamens zur Liste „TrustedHosts“ keine Aussage hinsichtlich der Vertrauenswürdigkeit des Hosts selbst beinhaltet, da das NTLM-Authentifizierungsprotokoll nicht garantieren kann, dass Sie tatsächlich eine Verbindung mit dem von Ihnen beabsichtigten Host herstellen.
Stattdessen sollten Sie die „TrustedHosts“-Einstellung als Liste der Hosts betrachten, für die Sie Fehler unterdrücken möchten, die generiert werden, weil die Identität des Servers nicht überprüft werden kann.
    
    
### Laufende Kommunikation

Nach Abschluss der anfänglichen Authentifizierung verschlüsselt das [PowerShell-Remotingprotokoll](https://msdn.microsoft.com/en-us/library/dd357801.aspx) die gesamte laufende Kommunikation mit einem sitzungsbezogenen, symmetrischen AES-256-Schlüssel.  


## Durchführen des zweiten Hops

Standardmäßig verwendet PowerShell-Remoting Kerberos (falls verfügbar) oder NTLM für die Authentifizierung. Beide Protokolle führen eine Authentifizierung gegenüber dem Remotecomputer durch, ohne Anmeldeinformationen an diesen zu senden.
Dies ist die sicherste Methode der Authentifizierung, doch da der Remotecomputer nicht über die Anmeldeinformationen des Benutzers verfügt, kann er nicht auf andere Computer und Dienste im Auftrag des Benutzers zugreifen. Dies ist bekannt als „Double-Hop“-Problem.

Es gibt verschiedene Möglichkeiten, um dieses Problem zu vermeiden:

### Eingeschränkte Kerberos-Delegierung

Für hochgradig vertrauenswürdige Server können Sie die [eingeschränkte Kerberos-Delegierung](https://technet.microsoft.com/en-us/library/cc995228.aspx) aktivieren. Dies gestattet es dem Remoteserver, die Identität des authentifizierten Benutzers gegenüber einer angegebenen Liste von Computern und Diensten anzunehmen.

### Vertrauensstellung zwischen Remotecomputern

Wenn Sie Benutzern vertrauen, die remote mit *Server1* und von da aus mit Ressourcen auf *Server2* verbunden sind, können Sie *Server1* explizit Zugriff auf diese Ressourcen erteilen.

### Verwenden expliziter Anmeldeinformationen beim Zugriff auf Remoteressourcen

Sie können Ihre Anmeldeinformationen explizit an eine Remoteressource übergeben, indem Sie den Parameter **Credential** eines Cmdlets verwenden. Beispiel:

```powershell
$myCredential = Get-Credential
New-PSDrive -Name Tools \\Server2\Shared\Tools -Credential $myCredential 
```

### CredSSP

Sie können den [Credential Security Support Provider (CredSSP)](https://msdn.microsoft.com/en-us/library/windows/desktop/bb931352.aspx) für die Authentifizierung verwenden (durch Angabe von „CredSSP“ als Wert des Parameters `Authentication` eines Aufrufs des Cmdlets [New-PSSession](https://technet.microsoft.com/en-us/library/hh849717.aspx)). CredSSP übergibt Anmeldeinformationen als Klartext an den Server, weshalb dessen Verwendung Sie gegenüber Angriffen zum Diebstahl von Anmeldeinformationen verwundbar macht. Wenn der Remotecomputer kompromittiert ist, hat der Angreifer Zugriff auf die Anmeldeinformationen des Benutzers. CredSSP ist standardmäßig sowohl auf Client- als auch auf Servercomputern deaktiviert. Sie sollten CredSSP nur in absolut vertrauenswürdigen Umgebungen aktivieren. Beispielsweise wenn ein Domänenadministrator eine Verbindung mit einem Domänencontroller herstellt, weil der Domänencontroller hochgradig vertrauenswürdig ist.

Weitere Informationen zu Sicherheitsaspekten bei der Verwendung von CredSSP für PowerShell-Remoting finden Sie unter [Versehentliche Sabotage: Vorsicht vor CredSSP](http://www.powershellmagazine.com/2014/03/06/accidental-sabotage-beware-of-credssp).

Weitere Informationen zu Angriffen zum Diebstahl von Anmeldeinformationen finden Sie unter [Abschwächen von Pass-the-Hash-Angriffen (PtH) und anderen Techniken zum Diebstahl von Anmeldeinformationen](https://www.microsoft.com/en-us/download/details.aspx?id=36036).











<!--HONumber=Oct16_HO1-->


