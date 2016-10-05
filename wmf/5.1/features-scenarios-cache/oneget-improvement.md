---
title: PackageManagement (alias OneGet)-Verbesserungen
contributor: jianyunt, quoctruong
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: bb1129e6aa20b64e94ddb6d7b7cf7b51b1df9ca3

---

>Hinweis: Vorschlag für einen aussagekräftigen Namen und eine Kurzbeschreibung bereitstellen

## PackageManagement (alias OneGet)-Verbesserungen ##
Im folgenden werden die Korrekturen beschrieben, die an WMF 5.1 erfolgt sind, um einige Probleme bei der Benutzererfahrung in der WMF-Version 5.0 zu beheben. 

1 **„-version“-Alias**

**Szenario**: Angenommen, Sie haben Version 1.0 und 2.0 des Pakets P1 auf Ihrem System installiert und möchten nun die Version 1.0 deinstallieren. Deshalb führen Sie „uninstall-package -name P1 -version 1.0“ aus. Sie erwarten, dass Version 1.0 nach dem Ausführen des Cmdlets deinstalliert wurde. Das Ergebnis ist jedoch, dass Version 2.0 deinstalliert wird. 
    
Die Ursache des Problems ist, dass der „-version“-Parameter ein Alias für den „-minimumversion“-Parameter ist. Wenn OneGet ein qualifiziertes Paket mit der Mindestversion 1.0 sucht, wird die neueste Version zurückgegeben. Dieses Verhalten wird normalerweise erwartet, da die meisten Benutzer die neueste Version finden wollen. Für den Fall „uninstall-package“ sollte dies aber nicht gelten.
    
**Lösung**: Der „-version“-Alias wurde aus OneGet und PowerShellGet vollständig entfernt. 

2 **Mehrere Aufforderungen zum Bootstrapping des NuGet-Anbieters**

**Szenario**: Wenn Sie „Find-Module“ oder „Install-Module“ oder andere OneGet-Cmdlets erstmals auf Ihrem Computer ausführen, versucht OneGet das Bootstrapping des NuGet-Anbieters. Das liegt daran, dass der PowerShellGet-Anbieter auch den NuGet-Anbieter verwendet, um PowerShell-Module herunterzuladen. OneGet fordert dann vom Benutzer die Berechtigung zum Installieren des NuGet-Anbieters an. Nachdem der Benutzer für das Bootstrapping „yes“ ausgewählt hat, wird die neueste Version des NuGet-Anbieters installiert. 
    
Wenn jedoch eine ältere Version des NuGet-Anbieters auf Ihrem Computer installiert ist, wird mitunter die ältere NuGet-Version zuerst in die PowerShell-Sitzung geladen (was die Racebedingung in OneGet ist). Damit PowerShellGet funktioniert, ist jedoch die neuere Version des NuGet-Anbieters erforderlich. Deshalb wird OneGet von PowerShellGet aufgefordert, für den NuGet-Anbieter erneut das Bootstrapping auszuführen. Deshalb werden mehrere Aufforderungen zum Bootstrapping des NuGet-Anbieters angezeigt.

**Lösung**: OneGet lädt nun die neueste Version des NuGet-Anbieters, um mehrere Aufforderungen zum Bootstrapping des NuGet-Anbieters zu vermeiden.

Es gibt auch eine Umgehung dieses Problems. Löschen Sie manuell die alte Version des NuGet-Anbieters (NuGet-Anycpu.exe), sofern vorhanden, aus „$env:ProgramFiles\PackageManagement\ProviderAssemblies $env:LOCALAPPDATA\PackageManagement\ProviderAssemblies“.


3 **Computer mit ausschließlich Intranetzugriff**

**Szenario**: Benutzer im Unternehmen haben keinen Zugriff auf das Internet, sondern nur auf das Intranet. Dies wurde von OneGet in WMF 5.0 nicht unterstützt.

**Lösung**:
- Sie können den NuGet-Anbieter auf einen anderen Computer mit Internetverbindung mithilfe des Befehls „Install-PackageProvider -Name NuGet“ herunterladen.

- Den soeben installierten NuGet-Anbieter finden Sie entweder unter „$env:ProgramFiles\PackageManagement\ProviderAssemblies\nuget“ oder „$env:LOCALAPPDATA\PackageManagement\ProviderAssemblies\nuget“. 

- Kopieren Sie die Binärdateien in einen Ordner oder eine Netzwerkfreigabe, auf die Ihr Computer (der ohne Internetzugang) zugreifen kann, und installieren Sie den NuGet-Anbieter mit „Install-PackageProvider NuGet -Source <Path to folder>“.


4 **Ereignisprotokoll**

Wenn Sie Pakete installieren, ändern Sie den Status Ihres Computers. Zu Diagnosezwecken protokolliert OneGet jetzt beim Installieren, Deinstallieren und Speichern von Paketen Ereignisse im Windows-Ereignisprotokoll. Der Ereigniskanal ist identisch mit PowerShell, d. h. Microsoft-Windows-PowerShell, Operational.

5 **Authentifizierungsunterstützung** OneGet unterstützt jetzt das Suchen und Installieren von Paketen aus einem Repository, das Standardauthentifizierung erfordert. Sie können Ihre Anmeldeinformationen den Cmdlets „Find-Package“ und „Install-Package“ angeben. Beispiel:
``` PowerShell
Find-Package -Source <SourceWithCredential> -Credential (Get-Credential)
```
6 **Verwenden von OneGet hinter einem Proxy**

OneGet verwendet jetzt die Proxyparameter „-ProxyCredential“ und „-Proxy“. Mithilfe dieser Parameter können Sie OneGet-Cmdlets die Proxy-ULR und Proxyanmeldeinformationen angeben (standardmäßig verwenden wir die Proxyeinstellungen des Systems). Beispiel:
``` PowerShell
Find-Package -Source http://www.nuget.org/api/v2/ -Proxy http://www.myproxyserver.com -ProxyCredential (Get-Credential)
```



<!--HONumber=Oct16_HO1-->


