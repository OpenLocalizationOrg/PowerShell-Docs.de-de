---
title: "Ausführen von Remotebefehlen"
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: d6938b56-7dc8-44ba-b4d4-cd7b169fd74d
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: d36a862d27ed4bb8a4bed2ae58f9479ba1b29d21

---

# Ausführen von Remotebefehlen
Mit einem einzigen Windows PowerShell-Befehl können Sie Befehle auf einem oder Hunderten von Computern ausführen. Windows PowerShell unterstützt das Remotecomputing mithilfe verschiedener Technologien, unter anderem WMI, RPC und WS-Management.

## Remoting ohne Konfiguration
Viele Windows PowerShell-Cmdlets verfügen über einen „ComputerName“-Parameter, mit dessen Hilfe Sie Daten auf einem oder mehreren Remotecomputern sammeln und Einstellungsänderungen vornehmen können. Sie verwenden eine Vielzahl von Kommunikationstechnologien und viele arbeiten ohne besondere Konfiguration unter allen Windows-Betriebssystemen, die Windows PowerShell unterstützen.

Zu diesem Cmdlets zählen:

-   [Computer neu starten](https://technet.microsoft.com/en-us/library/dd315301.aspx)

-   [Verbindung testen](https://technet.microsoft.com/en-us/library/dd315259.aspx)

-   [Clear-EventLog](https://technet.microsoft.com/en-us/library/dd347552.aspx)

-   [Get-EventLog](https://technet.microsoft.com/en-us/library/dd315250.aspx)

-   [Get-HotFix](https://technet.microsoft.com/en-us/library/e1ef636f-5170-4675-b564-199d9ef6f101)

-   [Get-Process](https://technet.microsoft.com/en-us/library/dd347630.aspx)

-   [Get-Service](https://technet.microsoft.com/en-us/library/dd347591.aspx)

-   [Set-Service](https://technet.microsoft.com/en-us/library/dd315324.aspx)

-   [Get-WinEvent](https://technet.microsoft.com/en-us/library/dd315358.aspx)

-   [Get-WmiObject](https://technet.microsoft.com/en-us/library/dd315295.aspx)

In der Regel weisen Cmdlets, die Remoting ohne besondere Konfiguration unterstützen, den „ComputerName“-Parameter und nicht den „Session“-Parameter auf. Um diese Cmdlets in Ihrer Sitzung zu finden, geben Sie Folgendes ein:

```
Get-Command | where { $_.parameters.keys -contains "ComputerName" -and $_.parameters.keys -notcontains "Session"}
```

## Windows PowerShell-Remoting
Mit dem Windows PowerShell-Remoting, bei dem das WS-Verwaltungsprotokoll verwendet wird, können Sie jeden Windows PowerShell-Befehl auf einem oder mehreren Remotecomputern ausführen. Sie können dauerhafte Verbindungen herstellen, interaktive 1:1-Sitzungen 1:1 starten und Skripts auf mehreren Computern ausführen.

Um Windows PowerShell-Remoting zu verwenden, muss der Remotecomputer für die Remoteverwaltung konfiguriert sein. Weitere Informationen und Anweisungen hierzu finden Sie unter [Informationen zu Remoteanforderungen](https://technet.microsoft.com/en-us/library/dd315349.aspx).

Nachdem Sie Windows PowerShell-Remoting konfiguriert haben, stehen Ihnen viele Remotingstrategien zur Verfügung. Im weiteren Verlauf dieses Dokuments sind einige davon aufgelistet. Weitere Informationen finden Sie unter [about_Remote](https://technet.microsoft.com/en-us/library/dd347744.aspx) und [about_Remote_FAQ](https://technet.microsoft.com/en-us/library/dd347744.aspx).

### Starten einer interaktiven Sitzung
Um eine interaktive Sitzung mit einem einzelnen Remotecomputer zu starten, verwenden Sie das Cmdlet [Enter-PSSession](https://technet.microsoft.com/en-us/library/dd315384.aspx). Um beispielsweise eine interaktive Sitzung mit dem Remotecomputer „Server01“ zu starten, geben Sie Folgendes ein:

```
Enter-PSSession Server01
```

Die Eingabeaufforderung ändert sich, und es wird der Name des Computers angezeigt, mit dem Sie verbunden sind. Von nun an werden alle Befehle, die Sie an der Eingabeaufforderung eingeben, auf dem Remotecomputer ausgeführt, und die Ergebnisse werden auf dem lokalen Computer angezeigt.

Um die interaktive Sitzung zu beenden, geben Sie Folgendes ein:

```
Exit-PSSession
```

Weitere Informationen über die Cmdlets „Enter-PSSession“ und „Exit-PSSession“ Sie unter [Enter-PSSession](https://technet.microsoft.com/en-us/library/dd315384.aspx) und [Exit-PSSession](https://technet.microsoft.com/en-us/library/dd315322.aspx).

### Ausführen eines Remotebefehls
Zum Ausführen eines Befehls auf einem oder mehreren Remotecomputern verwenden Sie das Cmdlet [Invoke-Command](https://technet.microsoft.com/en-us/library/dd347578.aspx).
Um beispielsweise einen [Get-UICulture](https://technet.microsoft.com/en-us/library/dd347742.aspx)-Befehl auf den Remotecomputern „Server01“ und „Server02“ auszuführen, geben Sie Folgendes ein:

```
Invoke-Command -ComputerName Server01, Server02 {Get-UICulture}
```

Die Ausgabe wird an den Computer zurückgegeben.

```
LCID    Name     DisplayName               PSComputerName
----    ----     -----------               --------------
1033    en-US    English (United States)   server01.corp.fabrikam.com
1033    en-US    English (United States)   server02.corp.fabrikam.com
```

Weitere Informationen zum Cmdlet „Invoke-Command“ finden Sie unter [Invoke-Command](https://technet.microsoft.com/en-us/library/22fd98ba-1874-492e-95a5-c069467b8462).

### Ausführen eines Skripts
Zum Ausführen eines Skripts auf einem oder mehreren Remotecomputern verwenden Sie den Parameter „FilePath“ des Cmdlets „Invoke-Command“. Das Skript muss sich auf dem lokalen Computer befinden oder für diesen verfügbar sein. Die Ergebnisse werden an den lokalen Computer zurückgegeben.

Der folgende Befehl führt beispielsweise das Skript „DiskCollect.ps1“ auf den Remotecomputern „Server01“ und „Server02“ aus.

```
Invoke-Command -ComputerName Server01, Server02 -FilePath c:\Scripts\DiskCollect.ps1
```

Weitere Informationen zum Cmdlet „Invoke-Command“ finden Sie unter [Invoke-Command](https://technet.microsoft.com/en-us/library/dd347578.aspx).

### Herstellen einer dauerhaften Verbindung
Um eine Reihe verwandter Befehle auszuführen, die Daten gemeinsam nutzen, erstellen Sie eine Sitzung auf dem Remotecomputer, und verwenden Sie dann das Cmdlet „Invoke-Command“ zum Ausführen der Befehle in der Sitzung, die Sie erstellen. Um eine Remotesitzung zu erstellen, verwenden Sie das Cmdlet „New-PSSession“.

Mit dem folgenden Befehl erstellen Sie z. B. eine Remotesitzung auf dem Computer "Server01" und eine weitere Remotesitzung auf dem Computer "Server02". Es speichert die Sitzungsobjekte in der Variablen „$s“.

```
$s = New-PSSession -ComputerName Server01, Server02
```

Nachdem die Sitzungen nun hergestellt sind, können Sie jeden beliebigen Befehl in diesen ausführen. Und da die Sitzungen dauerhaft sind, können Sie Daten in einem Befehl sammeln und diese in einem nachfolgenden Befehl verwenden.

Beispielsweise führt der folgende Befehl einen „Get-HotFix“-Befehl in den Sitzungen in der Variablen „$s“ aus und speichert die Ergebnisse dann in der Variablen „$h“. Die Variable „$h“ wird in jeder der Sitzungen in „$s“ erstellt, ist jedoch in der lokalen Sitzung nicht vorhanden.

```
Invoke-Command -Session $s {$h = Get-HotFix}
```

Sie können die Daten in der Variablen „$h“ jetzt in nachfolgenden Befehle verwenden, wie z. B. dem folgenden. Die Ergebnisse werden auf dem lokalen Computer angezeigt.

```
Invoke-Command -Session $s {$h | where {$_.installedby -ne "NTAUTHORITY\SYSTEM"}}
```

### Erweitertes Remoting
Dies sind nur die grundlegenden Möglichkeiten, die die Remoteverwaltung von Windows PowerShell bietet. Mithilfe von Cmdlets, die mit Windows PowerShell installiert werden, können Sie Remotesitzungen sowohl von lokaler Seite als auch von Remoteseite aus einrichten und konfigurieren, angepasste und eingeschränkte Sitzungen erstellen, Benutzern über eine Remotesitzung den Import von Befehlen ermöglichen, die tatsächlich implizit in der Remotesitzung ausgeführt werden, die Sicherheit einer Remotesitzung konfigurieren und vieles mehr.

Um die Remotekonfiguration zu vereinfachen, umfasst Windows PowerShell einen WSMan-Anbieter. Das vom Anbieter erstellte Laufwerk „WSMAN:“ ermöglicht Ihnen, durch eine Hierarchie von Konfigurationseinstellungen auf dem lokalen Computer und den Remotecomputern zu navigieren.
Weitere Informationen zum WSMan-Anbieter finden Sie unter [WSMan Provider](https://technet.microsoft.com/en-us/library/dd819476.aspx) und   [About_WS-Management_Cmdlets](https://technet.microsoft.com/en-us/library/dd819481.aspx), oder indem Sie in der Windows PowerShell-Konsole „Get-Help wsman“ eingeben.

Weitere Informationen finden Sie unter:
- [Informationen zu entfernten – häufig gestellte Fragen](https://technet.microsoft.com/en-us/library/dd315359.aspx)
- [Register-PSSessionConfiguration](https://technet.microsoft.com/en-us/library/dd819496.aspx)
- [Import-PSSession](https://technet.microsoft.com/en-us/library/dd347575.aspx). 

Hilfe zu Remotingfehlern finden Sie unter [about_Remote_Troubleshooting](https://technet.microsoft.com/en-us/library/dd347642.aspx).

## Weitere Informationen
- [about_Remote](https://technet.microsoft.com/en-us/library/9b4a5c87-9162-4adf-bdfe-fbc80b9b8970)
- [about_Remote_FAQ](https://technet.microsoft.com/en-us/library/e23702fd-9415-4a98-9975-390a4d3adc42)
- [about_Remote_Requirements](https://technet.microsoft.com/en-us/library/da213949-134c-4741-b307-81f4492ba1bd)
- [about_Remote_Troubleshooting](https://technet.microsoft.com/en-us/library/2f890148-8578-49ed-85ea-79a489dd6317)
- ["about_pssessions"](https://technet.microsoft.com/en-us/library/7a9b4e0e-fa1b-47b0-92f6-6e2995d70acb)
- [About_WS Management_Cmdlets](https://technet.microsoft.com/en-us/library/6ed3370a-ea10-45a5-9493-696aeace27ed)
- [Invoke-Command](https://technet.microsoft.com/en-us/library/22fd98ba-1874-492e-95a5-c069467b8462)
- [Import-PSSession](https://technet.microsoft.com/en-us/library/048c115e-a6fb-4e0d-8cea-c5ca24630c9d)
- [New-PSSession](https://technet.microsoft.com/en-us/library/59452f12-a11d-4558-99ea-e6ca6ad5ffd3)
- [Register-PSSessionConfiguration](https://technet.microsoft.com/en-us/library/af68867a-d201-4b19-a1de-594015ed8a25)
- [WSMan-Anbieter](https://technet.microsoft.com/en-us/library/66fe1241-e08f-49ca-832f-a84c33ca8735)




<!--HONumber=Oct16_HO1-->


