---
title: "Ausführen von Netzwerkaufgaben"
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: a43cc55f-70c1-45c8-9467-eaad0d57e3b5
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 39266e1e4ae2101de26277c20a98596f62cf223d

---

# Ausführen von Netzwerkaufgaben
Weil TCP/IP das am häufigsten verwendete Netzwerkprotokoll ist, geht es bei den meisten Aufgaben zur grundlegenden Netzwerkprotokollverwaltung um TCP/IP. In diesem Abschnitt werden Windows PowerShell und WMI verwendet, um diese Aufgaben auszuführen.

### Auflisten der IP-Adressen für einen Computer
Um alle IP-Adressen abzurufen, die auf dem lokalen Computer verwendet werden, verwenden Sie den folgenden Befehl:

```
Get-WmiObject -Class Win32_NetworkAdapterConfiguration -Filter IPEnabled=TRUE -ComputerName . | Format-Table -Property IPAddress
```

Die Ausgabe dieses Befehls unterscheidet sich von den meisten Eigenschaftenlisten, weil die jeweiligen Werte in geschweiften Klammern stehen:

<pre>IPAddress
---------
{192.168.1.80} {192.168.148.1} {192.168.171.1} {0.0.0.0}</pre>

Um zu verstehen, warum die geschweiften Klammern angezeigt werden, verwenden Sie das Cmdlet „Get-Member“, um sich die **IPAddress**-Eigenschaft anzusehen:

<pre>PS> Get-WmiObject -Class Win32_NetworkAdapterConfiguration -Filter IPEnabled=TRUE -ComputerName . | Get-Member -Name IPAddress TypeName: System.Management.ManagementObject#root\cimv2\Win32_NetworkAdapter Configuration Name      MemberType Definition ----      ---------- ---------- IPAddress Property   System.String[] IPAddress {get;}</pre>

Die „IPAdress“-Eigenschaft für jede Netzwerkkarte ist tatsächlich ein Array. Die geschweiften Klammern in der Definition kennzeichnen, dass **IPAdress** ein **System.String**-Wert ist, allerdings ein Array von **System.String**-Werten.

### Auflisten von IP-Konfigurationsdaten
Um die detaillierten IP-Konfigurationsdaten für jede Netzwerkkarte anzuzeigen, verwenden Sie den folgenden Befehl:

```
Get-WmiObject -Class Win32_NetworkAdapterConfiguration -Filter IPEnabled=TRUE -ComputerName .
```

Die Standardanzeige für das Konfigurationsobjekt einer Netzwerkkarte ist ein sehr eingeschränkter Satz der verfügbaren Informationen. Für eine ausführliche Überprüfung und Problembehandlung verwenden Sie das Cmdlet „Select-Object“ oder ein Formatierungs-Cmdlet, etwa „Format-List“, um die anzuzeigenden Eigenschaften anzugeben.

Wenn Sie nicht an den IPX- oder WINS-Eigenschaften interessiert sind – was in einem modernen TCP/IP-Netzwerk wahrscheinlich der Fall ist –, können Sie den „ExcludeProperty“-Parameter von „Select-Object“ verwenden, um Eigenschaften auszublenden, deren Namen mit „WINS“ oder „IPX“ beginnen:

```
Get-WmiObject -Class Win32_NetworkAdapterConfiguration -Filter IPEnabled=TRUE -ComputerName . | Select-Object -Property [a-z]* -ExcludeProperty IPX*,WINS*
```

Dieser Befehl gibt detaillierte Informationen zu DHCP, DNS, Routing und einigen unwesentlicheren IP-Konfigurationseigenschaften zurück.

### Pingen von Computern
Sie können einen einfachen Ping zu einem Computer ausführen, indem Sie **Win32_PingStatus** verwenden. Der folgende Befehl führt den Ping aus, gibt jedoch eine ausführliche Ausgabe zurück:

```
Get-WmiObject -Class Win32_PingStatus -Filter "Address='127.0.0.1'" -ComputerName .
```

Eine nützlichere Form für Zusammenfassungsinformationen ist eine Anzeige der Eigenschaften „Address“, „ResponseTime“ und „StatusCode“, wie sie vom folgenden Befehl generiert wird. Der „Autosize“-Parameter von „Format-Table“ bewirkt eine Breitenänderung der Tabellenspalten, sodass diese ordnungsgemäß in Windows PowerShell angezeigt werden.

```
PS> Get-WmiObject -Class Win32_PingStatus -Filter "Address='127.0.0.1'" -ComputerName . | Format-Table -Property Address,ResponseTime,StatusCode -Autosize

Address   ResponseTime StatusCode
-------   ------------ ----------
127.0.0.1            0          0
A status code of 0 indicates a successful ping.
```

Sie können ein Array verwenden, um mehrere Computer mit einem einzigen Befehl zu pingen. Da es mehrere Adressen gibt, verwenden Sie das Cmdlet **ForEach-Object**, um jede Adresse einzeln zu pingen:

```
"127.0.0.1","localhost","research.microsoft.com" | ForEach-Object -Process {Get-WmiObject -Class Win32_PingStatus -Filter ("Address='" + $_ + "'") -ComputerName .} | Select-Object -Property Address,ResponseTime,StatusCode
```

Mit dem gleichen Befehlsformat können Sie alle Computer in einem Subnetz pingen, beispielsweise in einem privaten Netzwerk, in dem die Netzwerknummer 192.168.1.0 und eine standardmäßige Klasse-C-Subnetzmaske (255.255.255.0) verwendet wird. Nur Adressen im Bereich von 192.168.1.1 bis 192.168.1.254 sind zulässige lokale Adressen (0 ist immer für die Netzwerknummer reserviert, und 255 ist eine Subnetz-Broadcastadresse).

Um ein Array der Zahlen von 1 bis 254 in Windows PowerShell darzustellen, verwenden Sie die Anweisung **1..254**. Ein vollständiges Subnetz-Ping kann ausgeführt werden, indem das Array erstellt und dann jeder Wert zu einer unvollständigen Adresse in der Ping-Anweisung hinzugefügt wird:

```
1..254| ForEach-Object -Process {Get-WmiObject -Class Win32_PingStatus -Filter ("Address='192.168.1." + $_ + "'") -ComputerName .} | Select-Object -Property Address,ResponseTime,StatusCode
```

Diese Vorgehensweise zum Generieren eines Adressbereichs kann auch an jeder anderen Stelle verwendet werden. Sie können einen vollständigen Satz von Adressen auf diese Weise generieren:

`$ips = 1..254 | ForEach-Object -Process {"192.168.1." + $_}`

### Abrufen von Netzwerkkarteneigenschaften
Wie bereits in diesem Handbuch erläutert wurde, können Sie allgemeine Konfigurationseigenschaften über **Win32_NetworkAdapterConfiguration** abrufen. Netzwerkkarteninformationen wie MAC-Adressen und Kartentypen sind zwar keine TCP/IP-Informationen, können aber trotzdem nützlich sein, um zu verstehen, was mit einem Computer los ist. Um eine Zusammenfassung dieser Informationen zu erhalten, verwenden Sie den folgenden Befehl:

```
Get-WmiObject -Class Win32_NetworkAdapter -ComputerName .
```

### Zuweisen der DNS-Domäne für eine Netzwerkkarte
Wenn Sie die DNS-Domäne für automatische Namensauflösung zuweisen möchten, verwenden Sie die **Win32_NetworkAdapterConfiguration SetDNSDomain**-Methode. Weil Sie die DNS-Domäne für jede Netzwerkkartenkonfiguration unabhängig zuweisen, müssen Sie eine **ForEach-Object**-Anweisung verwenden, um die Domäne jedem Adapter zuzuweisen:

```
Get-WmiObject -Class Win32_NetworkAdapterConfiguration -Filter IPEnabled=true -ComputerName . | ForEach-Object -Process { $_. SetDNSDomain("fabrikam.com") }
```

Die Filteranweisung „IPEnabled=True“ ist erforderlich, da selbst in einem Netzwerk, in dem nur TCP/IP verwendet wird, verschiedene Netzwerkkartenkonfigurationen auf einem Computer keine wirklichen TCP/IP-Karten sind. Sie sind allgemeine Softwareelemente, die RAS, PPTP, QoS und andere Dienste für alle Karten unterstützen und daher keine eigenen Adressen haben.

Sie können den Befehl filtern, indem Sie das Cmdlet **Where-Object** anstelle des **Get-WmiObject**-Filters verwenden.

```
Get-WmiObject -Class Win32_NetworkAdapterConfiguration -ComputerName . | Where-Object -FilterScript {$_.IPEnabled} | ForEach-Object -Process {$_.SetDNSDomain("fabrikam.com")}
```

### Ausführen von DHCP-Konfigurationsaufgaben
Ein Ändern von DHCP-Details umfasst das Arbeiten mit einem Satz von Netzwerkkarten, ähnlich wie dies bei der DNS-Konfiguration der Fall ist. Es gibt mehrere unterschiedliche Aktionen, die Sie über WMI ausführen können. Einige der üblichen Aktionen werden hier durchlaufen.

#### Ermitteln der DHCP-fähigen Netzwerkkarten
Um die DHCP-fähigen Netzwerkkarten auf einem Computer zu ermitteln, verwenden Sie den folgenden Befehl:

```
Get-WmiObject -Class Win32_NetworkAdapterConfiguration -Filter "DHCPEnabled=true" -ComputerName .
```

Wenn Sie Netzwerkkarten mit IP-Konfigurationsproblemen ausschließen möchten, können Sie so vorgehen, dass Sie nur IP-fähige Netzwerkkarten abrufen:

```
Get-WmiObject -Class Win32_NetworkAdapterConfiguration -Filter "IPEnabled=true and DHCPEnabled=true" -ComputerName .
```

#### Abrufen von DHCP-Eigenschaften
Da DHCP-bezogene Eigenschaften für eine Netzwerkkarte grundsätzlich mit „DHCP“ beginnen, können Sie den „Property“-Parameter von „Format-Table“ verwenden, um ausschließlich diese Eigenschaften anzuzeigen:

```
Get-WmiObject -Class Win32_NetworkAdapterConfiguration -Filter "DHCPEnabled=true" -ComputerName . | Format-Table -Property DHCP*
```

#### Aktivieren von DHCP auf jeder Netzwerkkarte
Wenn Sie DHCP auf allen Netzwerkkarten aktivieren möchten, verwenden Sie den folgenden Befehl:

```
Get-WmiObject -Class Win32_NetworkAdapterConfiguration -Filter IPEnabled=true -ComputerName . | ForEach-Object -Process {$_.EnableDHCP()}
```

Sie können die **Filter**-Anweisung „IPEnabled=true and DHCPEnabled=false“ verwenden, um ein Aktivieren von DHCP dort zu vermeiden, wo es bereits aktiviert ist, wobei keine Fehler verursacht werden, wenn dieser Schritt nicht erfolgt.

#### Freigeben und Erneuern von DHCP-Leases auf bestimmten Netzwerkkarten
Die **Win32_NetworkAdapterConfiguration**-Klasse hat die Methoden **ReleaseDHCPLease** und **RenewDHCPLease**. Beide werden auf die gleiche Weise verwendet. Üblicherweise verwenden Sie diese Methoden, wenn Sie nur Adressen für eine Netzwerkkarte in einem bestimmten Subnetz freigeben oder erneuern müssen. Die einfachste Möglichkeit zum Filtern von Netzwerkkarten in einem Subnetz besteht darin, nur die Netzwerkkartenkonfigurationen auswählen, die das Gateway für das Subnetz verwenden. Beispielsweise gibt der folgende Befehl alle DHCP-Leases auf Netzwerkkarten des lokalen Computers frei, die DHCP-Leases von 192.168.1.254 erhalten:

```
Get-WmiObject -Class Win32_NetworkAdapterConfiguration -Filter "IPEnabled=true and DHCPEnabled=true" -ComputerName . | Where-Object -FilterScript {$_.DHCPServer -contains "192.168.1.254"} | ForEach-Object -Process {$_.ReleaseDHCPLease()}
```

Die einzige Änderung für die Erneuerung einer DHCP-Lease ist die, dass die **RenewDHCPLease**-Methode anstelle der **ReleaseDHCPLease**-Methode verwendet wird:

```
Get-WmiObject -Class Win32_NetworkAdapterConfiguration -Filter "IPEnabled=true and DHCPEnabled=true" -ComputerName . | Where-Object -FilterScript {$_.DHCPServer -contains "192.168.1.254"} | ForEach-Object -Process {$_.ReleaseDHCPLease()}
```

> [!NOTE]
> Wenn Sie diese Methoden auf einem Remotecomputer verwenden, sollten Sie daran denken, dass der Zugriff auf das Remotesystem verloren gehen kann, wenn die Verbindung mit dem System über die Netzwerkkarte mit der freigegebenen oder erneuerten Lease besteht.

#### Freigeben und Erneuern von DHCP-Leases auf allen Netzwerkkarten
Sie können globale DHCP-Adressfreigaben oder -erneuerungen auf allen Netzwerkkarten ausführen, indem Sie die **Win32_NetworkAdapterConfiguration**-Methoden **ReleaseDHCPLeaseAll** und **RenewDHCPLeaseAll** verwenden. Allerdings muss der Befehl auf die WMI-Klasse statt auf eine bestimmte Netzwerkkarte angewendet werden, weil globales Freigeben und Erneuern von Leases für die Klasse, nicht für eine bestimmte Netzwerkkarte ausgeführt wird.

Sie können einen Verweis auf eine WMI-Klasse anstelle von Klasseninstanzen abrufen, indem Sie alle WMI-Klassen auflisten und dann nur die gewünschte Klasse nach Name auswählen. Beispielsweise gibt der folgende Befehl die „Win32_NetworkAdapterConfiguration“-Klasse zurück:

```
Get-WmiObject -List | Where-Object -FilterScript {$_.Name -eq "Win32_NetworkAdapterConfiguration"}
```

Sie können den gesamten Befehl als die Klasse behandeln und dann die **ReleaseDHCPAdapterLease**-Methode für ihn aufrufen. Im folgenden Befehl weisen die Klammern, die die Pipelineelemente **Get-WmiObject** und **Where-Object** umschließen, Windows PowerShell an, diese zuerst auszuwerten:

```
( Get-WmiObject -List | Where-Object -FilterScript {$_.Name -eq "Win32_NetworkAdapterConfiguration"} ).ReleaseDHCPLeaseAll()
```

Sie können das gleiche Befehlsformat verwenden, um die **RenewDHCPLeaseAll**-Methode aufzurufen:

```
( Get-WmiObject -List | Where-Object -FilterScript {$_.Name -eq "Win32_NetworkAdapterConfiguration"} ).RenewDHCPLeaseAll()
```

### Erstellen einer Netzwerkfreigabe
Um eine Netzwerkfreigabe zu erstellen, verwenden Sie die **Win32_Share Create**-Methode:

```
(Get-WmiObject -List -ComputerName . | Where-Object -FilterScript {$_.Name -eq "Win32_Share"}).Create("C:\temp","TempShare",0,25,"test share of the temp folder")
```

Sie können die Freigabe auch erstellen, indem Sie **net share** in Windows PowerShell verwenden:

```
net share tempshare=c:\temp /users:25 /remark:"test share of the temp folder"
```

### Entfernen einer Netzwerkfreigabe
Sie können eine Netzwerkfreigabe mit **Win32_Share** entfernen, aber der Vorgang unterscheidet sich etwas vom Erstellen einer Freigabe, weil Sie die bestimmte zu entfernende Freigabe anstelle der **Win32_Share**-Klasse abrufen müssen. Mit der folgenden Anweisung wird die Freigabe „TempShare“ gelöscht:

```
(Get-WmiObject -Class Win32_Share -ComputerName . -Filter "Name='TempShare'").Delete()
```

**net share** funktioniert ebenfalls:

```
PS> net share tempshare /delete
tempshare was deleted successfully.
```

### Verbinden eines für Windows verfügbaren Netzlaufwerks
Das Cmdlet **New-PSDrive** erstellt ein Windows PowerShell-Laufwerk, aber Laufwerke, die auf diese Weise erstellt wurden, sind nur für Windows PowerShell verfügbar. Um ein neues Netzlaufwerk zu erstellen, können Sie das **WScript.Network**-COM-Objekt verwenden. Im folgenden Befehl wird die Freigabe „\\FPS01\users“ dem lokalen Laufwerk B: zugeordnet:

```
(New-Object -ComObject WScript.Network).MapNetworkDrive("B:", "\\FPS01\users")
```

Der Befehl **net use** funktioniert ebenfalls:

```
net use B: \\FPS01\users
```

Laufwerke, die entweder mit **WScript.Network** oder „net use“ zugeordnet wurden, sind sofort für Windows PowerShell verfügbar.




<!--HONumber=Oct16_HO1-->


