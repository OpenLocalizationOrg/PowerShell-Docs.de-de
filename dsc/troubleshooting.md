#Problembehandlung bei DSC

> Gilt für: WindowsPowerShell 4.0, WindowsPowerShell 5.0

Dieses Thema beschreibt die Methoden zum Abrufen der gewünschten State Configuration (DSC) Skripts ohne Fehler ausgeführt. Protokolle effektiv mit werde zum Aufspüren von Fehlern und verstehen, wie wiederzuverwenden, den Cache, um die Ressource ändert, die unmittelbaren Ergebnisse finden Sie unter Sie DSC effektiver behandeln können. Diese Technics werden in zwei Abschnitten behandelt:

* Das Skript nicht ausgeführt: **mithilfe von DSC Skriptfehler Diagnose protokolliert**
* Meine Ressourcen nicht aktualisiert: **Zurücksetzen des Caches**

##Das Skript nicht ausgeführt: DSC mithilfe von meldet zur diagnose von Fehlern

Wie bei allen Windows Software DSC zeichnet Fehler und Ereignisse in [Protokolle](https://msdn.microsoft.com/library/windows/desktop/aa363632.aspx) können angezeigt werden, aus der [Ereignisanzeige](http://windows.microsoft.com/windows/what-information-event-logs-event-viewer). Untersuchen diese Protokolle kann Ihnen helfen zu verstehen, warum ein bestimmter Vorgang fehlgeschlagen ist und wie Sie Fehler in der Zukunft zu vermeiden. Schreiben von Konfigurationsskripts können problematisch werden, damit Überwachung Fehler einfacher als Sie den Autor, verwenden Sie die DSC-Protokoll-Ressource, um den Fortschritt der Konfiguration im Ereignisprotokoll DSC analytische.

##Wo DSC Ereignisprotokolle sind?

In der Ereignisanzeige DSC-Ereignisse, die sind: **Applications and Services Protokolle/Microsoft/Windows/gewünschten Konfiguration**

Das entsprechende PowerShell-Cmdlet [Get-WinEvent](https://technet.microsoft.com/library/hh849682.aspx), kann auch ausgeführt werden, um die Ereignisprotokolle anzuzeigen:

```
PS C:\Users> Get-WinEvent -LogName "Microsoft-Windows-Dsc/Operational"
   ProviderName: Microsoft-Windows-DSC
TimeCreated                     Id LevelDisplayName Message                                                                                                  
-----------                     -- ---------------- -------                                                                                                  
11/17/2014 10:27:23 PM        4102 Information      Job {02C38626-D95A-47F1-9DA2-C1D44A7128E7} : 
```

Wie oben gezeigt, die DSC primären Protokollname **Microsoft -> Windows -> DSC** (andere Namen unter Windows werden hier nicht dargestellt aus Platzgründen). Der primäre Name wird auf den Kanalnamen zum Erstellen des vollständigen Protokollnamens angefügt. Das Modul DSC schreibt, hauptsächlich in drei Arten von Protokollen: [betriebsbereit, analytische und Debugprotokolle](https://technet.microsoft.com/library/cc722404.aspx). Da die analytische und Debugprotokolle sind standardmäßig deaktiviert, sollten Sie sie in der Ereignisanzeige aktivieren. Öffnen Sie hierzu Ereignisanzeige Eventvwr in Windows PowerShell eingeben. Klicken Sie auf die **Starten** klicken Sie **Control Panel**, klicken Sie auf **Verwaltung**, und klicken Sie dann auf **Ereignisanzeige**. Auf der **Ansicht** klicken Sie im Menü in der Ereignisanzeige auf **Anzeigen analytische und Debugprotokolle**. Der Name des für den analytischen Kanal ist **Microsoft-Windows-Dsc/analytische**, und der Debug-Kanal ist **Microsoft-Windows-Dsc/Debug**. Außerdem können Sie die [Wevtutil](https://technet.microsoft.com/library/cc732848.aspx) Utility für die Protokolle aktivieren, wie im folgenden Beispiel gezeigt.

```powershell
wevtutil.exe set-log “Microsoft-Windows-Dsc/Analytic” /q:true /e:true
```

##Was enthalten DSC-Protokolle?

DSC-Protokolle sind über die drei protokollkanäle, die basierend auf der Wichtigkeit der Meldung aufgeteilt. Das Betriebsprotokoll in DSC enthält alle Fehlermeldungen und kann verwendet werden, um ein Problem zu identifizieren. Das analytische Protokoll verfügt über eine höhere Anzahl von Ereignissen und identifizieren, wo der Fehler aufgetreten ist. Dieser Kanal enthält auch ausführliche Meldungen (sofern vorhanden). Das Debugprotokoll enthält Protokolle, mit denen Sie verstehen, wie der Fehler aufgetreten ist. DSC-Ereignisnachrichten sind so strukturiert, dass jede Ereignisnachricht eine Auftrags-ID beginnt, die eindeutig einen DSC Vorgang darstellt. Das folgende Beispiel versucht, die Nachricht aus dem ersten Ereignis DSC Betriebsprotokoll angemeldet zu erhalten.

```powershell
PS C:\Users> $AllDscOpEvents=get-winevent -LogName "Microsoft-Windows-Dsc/Operational"
PS C:\Users> $FirstOperationalEvent=$AllDscOpEvents[0]
PS C:\Users> $FirstOperationalEvent.Message
Job {02C38626-D95A-47F1-9DA2-C1D44A7128E7} : 
Consistency engine was run successfully. 
```

DSC-Ereignisse werden in einer bestimmten Struktur protokolliert, die den Benutzer Ereignisse aus einem DSC Auftrag ermöglicht. Die Struktur lautet wie folgt:

**Auftrags-ID: \ < Guid\ >**
**\ < Event Message\ >**

##Sammeln von Ereignissen aus einem einzelnen DSC-Vorgang

DSC-Ereignisprotokollen enthalten Ereignisse, die von verschiedenen DSC-Vorgänge generiert. Allerdings werden Sie in der Regel mit den Details über nur einen bestimmten Vorgang betroffenen sein. Durch die Auftrags-ID-Eigenschaft, die für jeden Vorgang DSC eindeutig ist, können alle DSC Protokolle gruppiert werden. Die Auftrags-ID wird als der erste Eigenschaftswert in allen DSC-Ereignissen angezeigt. Die folgenden Schritte wird erläutert, wie alle Ereignisse in einem gruppierten Arraystruktur zu sammeln.

```powershell
<##########################################################################
 Step 1 : Enable analytic and debug DSC channels (Operational channel is enabled by default)
###########################################################################>

wevtutil.exe set-log “Microsoft-Windows-Dsc/Analytic” /q:true /e:true
wevtutil.exe set-log “Microsoft-Windows-Dsc/Debug” /q:True /e:true

<##########################################################################
 Step 2 : Perform the required DSC operation (Below is an example, you could run any DSC operation instead)
###########################################################################>

Get-DscLocalConfigurationManager

<##########################################################################
Step 3 : Collect all DSC Logs, from the Analytic, Debug and Operational channels
###########################################################################>

$DscEvents=[System.Array](Get-WinEvent "Microsoft-windows-dsc/operational") `
         + [System.Array](Get-WinEvent "Microsoft-Windows-Dsc/Analytic" -Oldest) `
         + [System.Array](Get-Winevent "Microsoft-Windows-Dsc/Debug" -Oldest)


<##########################################################################
 Step 4 : Group all logs based on the job ID
###########################################################################>
$SeparateDscOperations=$DscEvents | Group {$_.Properties[0].value}  
```

Hier wird die Variable `$SeparateDscOperations` Protokolle gruppiert nach Auftrags-IDs enthält. Jedes Arrayelement diese Variable stellt eine Gruppe von Ereignissen, die von einem anderen DSC-Vorgang ermöglicht den Zugriff auf Weitere Informationen zu den Ereignisprotokollen protokolliert werden.

```
PS C:\> $SeparateDscOperations

Count Name                      Group                                                                     
----- ----                      -----                                                                     
   48 {1A776B6A-5BAC-11E3-BF... {System.Diagnostics.Eventing.Reader.EventLogRecord, System.Diagnostics....
   40 {E557E999-5BA8-11E3-BF... {System.Diagnostics.Eventing.Reader.EventLogRecord, System.Diagnostics....
PS C:\> $SeparateDscOperations[0].Group
   ProviderName: Microsoft-Windows-DSC
TimeCreated                     Id LevelDisplayName Message                                               
-----------                     -- ---------------- -------                                               
12/2/2013 3:47:29 PM          4115 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4198 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4114 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4102 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4098 Warning          Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4098 Warning          Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4176 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4182 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4182 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4182 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4182 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4182 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4182 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4182 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4182 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...       
```

Sie können die Daten in die Variable extrahieren `$SeparateDscOperations` mit [Where-Object](https://technet.microsoft.com/library/ee177028.aspx). Im folgenden finden fünf Szenarios, in denen Sie zum Extrahieren von Daten für die Problembehandlung bei DSC sollten:

###1: Fehler bei

Alle Ereignisse verfügen über [Schweregrade] (https://msdn.microsoft.com/library/dd996917 (v=vs.85)). Diese Informationen kann verwendet werden, um die Fehlerereignisse zu identifizieren:

```
PS C:\> $SeparateDscOperations  | Where-Object {$_.Group.LevelDisplayName -contains "Error"}
Count Name                      Group                                                                     
----- ----                      -----                                                                     
   38 {5BCA8BE7-5BB6-11E3-BF... {System.Diagnostics.Eventing.Reader.EventLogRecord, System.Diagnostics....
```

###2: Details Vorgänge ausführen, in der letzten halben Stunde

`TimeCreated`, eine Eigenschaft für jedes Windows-Ereignis gibt die Zeit, die das Ereignis erstellt wurde. Vergleichen diese Eigenschaft mit einem bestimmten Datum/Uhrzeit-Objekt kann verwendet werden, um alle Ereignisse zu filtern:

```powershell
PS C:\> $DateLatest=(Get-date).AddMinutes(-30)
PS C:\> $SeparateDscOperations  | Where-Object {$_.Group.TimeCreated -gt $DateLatest}
Count Name                      Group                                                                     
----- ----                      -----                                                                     
    1 {6CEC5B09-5BB0-11E3-BF... {System.Diagnostics.Eventing.Reader.EventLogRecord}   
```

###3: Nachrichten aus dem aktuellen Vorgang

Der aktuelle Vorgang befindet sich in den ersten Index aus der Gruppe "Array" `$SeparateDscOperations`. Abfragen von Nachrichten von der Gruppe für den Index 0, gibt alle Nachrichten für den aktuellen Vorgang zurück:

```powershelll
PS C:\> $SeparateDscOperations[0].Group.Message
Job {5BCA8BE7-5BB6-11E3-BF41-00155D553612} : 
Running consistency engine.
Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : 
Configuration is sent from computer NULL by user sid S-1-5-18.
Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : 
Displaying messages from built-in DSC resources:
 WMI channel 1 
 ResourceID:  
 Message : [INCH-VM]:                            [] Starting consistency engine.
Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : 
Displaying messages from built-in DSC resources:
 WMI channel 1 
 ResourceID:  
 Message : [INCH-VM]:                            [] Consistency check completed. 
```

###4: Fehlermeldungen, die für die letzte fehlgeschlagene Vorgänge protokolliert

`$SeparateDscOperations [0]. Gruppe` enthält eine Reihe von Ereignissen für den aktuellen Vorgang. Führen Sie die `Where-Object` Cmdlet, um die Ereignisse zu Filtern anhand ihrer Namen anzeigen. Ergebnisse werden gespeichert, der `$myFailedEvent` Variable, um die ereignismeldung erhalten geschnitten die weitergegeben werden können:

```powershell
PS C:\> $myFailedEvent=($SeparateDscOperations[0].Group | Where-Object {$_.LevelDisplayName -eq "Error"})

PS C:\> $myFailedEvent.Message
Job {5BCA8BE7-5BB6-11E3-BF41-00155D553612} : 
DSC Engine Error : 
 Error Message Current configuration does not exist. Execute Start-DscConfiguration command with -Path pa
rameter to specify a configuration file and create a current configuration first. 
Error Code : 1 
```

###5: aller Ereignisse, die für einen bestimmten Auftrags-ID.

`$SeparateDscOperations` ist ein Array von Gruppen, von denen jeder Namen wie die eindeutige Auftrags-ID. hat Durch Ausführen der `Where-Object` -Cmdlet können Sie diese Gruppen von Ereignissen, die eine bestimmten Auftrag-ID extrahieren:

```powershell
PS C:\> ($SeparateDscOperations | Where-Object {$_.Name -eq $jobX} ).Group

   ProviderName: Microsoft-Windows-DSC

TimeCreated                     Id LevelDisplayName Message                                               
-----------                     -- ---------------- -------                                               
12/2/2013 4:33:24 PM          4102 Information      Job {847A5619-5BB2-11E3-BF41-00155D553612} : ...      
12/2/2013 4:33:24 PM          4168 Information      Job {847A5619-5BB2-11E3-BF41-00155D553612} : ...      
12/2/2013 4:33:24 PM          4146 Information      Job {847A5619-5BB2-11E3-BF41-00155D553612} : ...      
12/2/2013 4:33:24 PM          4120 Information      Job {847A5619-5BB2-11E3-BF41-00155D553612} : ...  
```

##Verwenden von xDscDiagnostics zur Analyse von DSC protokolliert

**xDscDiagnostics** ist ein PowerShell-Modul, das besteht aus zwei einfache Vorgänge, mit denen DSC-Fehler auf Ihrem Computer zu analysieren: `Get-xDscOperation` und `Trace-xDscOperation`. Diese Funktionen können Sie alle lokalen Ereignisse aus vergangenen DSC Vorgänge oder DSC-Ereignisse auf Remotecomputern (mit gültigen Anmeldeinformationen) zu identifizieren. Hier wird der Begriff DSC-Vorgang wird verwendet, um eine einzelne eindeutige DSC Ausführung von dessen Anfang bis Ende zu definieren. Beispielsweise `Test DscConfiguration` wäre eine separate DSC-Vorgang. Auf ähnliche Weise alle anderen Cmdlet in DSC (z. B. `Get-DscConfiguration`, `Start DscConfiguration`, usw.) konnte jeweils als separate DSC-Vorgänge identifiziert werden. Die zwei Cmdlets werden erläutert [xDscDiagnostics](https://powershellgallery.com/packages/xDscDiagnostics) PowerShell-Modul (DSC Resource Kit) und im folgenden ausführlich. Hilfe ist verfügbar durch Ausführen von `Get-Help < Cmdlet-Name >`.

##Get-xDscOperation

Dieses Cmdlet ermöglicht Ihnen, die Ergebnisse der DSC-Vorgänge zu suchen, die auf einem oder mehreren Computern ausgeführt und gibt ein Objekt, das die Auflistung von jeder DSC-Vorgang erzeugten Ereignisse enthält. In der folgenden Ausgabe wurden beispielsweise drei Befehle ausführen. Übergeben der ersten und den anderen zwei nicht. Diese Ergebnisse werden in der Ausgabe der zusammengefasst `Get-xDscOperation`.

TODO: Ersetzen Sie dieses Bild an, die Ausgabe von Get-xDscOperation zeigt

###Parameter

* **Neueste**: akzeptiert einen ganzzahligen Wert an die Anzahl der Vorgänge angezeigt werden. Standardmäßig wird die 10 neuesten Vorgänge zurückgegeben. Zum Beispiel
   TODO: Anzeigen von Get-xDscOperation-neueste 5
* **ComputerName**: Parameter, der ein Array von Zeichenfolgen jede enthält den Namen eines Computers akzeptiert aus, in dem Sie DSC Event Log-Daten sammeln möchten. Standardmäßig werden Daten vom Host-Computer erfasst. Um dieses Feature zu aktivieren, müssen Sie den folgenden Befehl ausführen, in den Remotecomputern im erweiterten Modus, damit die Auflistung von Ereignissen ermöglicht
```powershell
  New-NetFirewallRule -Name "Service RemoteAdmin" -Action Allow
```
* **Anmeldeinformationen**: Geben Sie Parameter, die vom PS, der Zugriff auf die Computer in den ComputerName-Parameter angegebene unterstützen kann.

###Zurückgegebene Objekt

Das Cmdlet gibt ein Array von Objekten jeweils Typ **Microsoft.PowerShell.xDscDiagnostics.GroupedEvents**. Jedes Objekt in diesem Array bezieht sich auf einen anderen DSC-Vorgang. Die Standardanzeige für dieses Objekt hat die folgenden Eigenschaften
* **SequenceID**: Gibt die inkrementelle Nummer der DSC-Vorgang basierend auf der Zeit. Z. B. der letzte ausgeführte Vorgang müssten SequenceID als 1, würde das zweite in letzter DSC-Vorgang haben SequenceID von 2 und so weiter. Diese Zahl ist, einen anderen Bezeichner für jedes Objekt im zurückgegebenen Array.
* **TimeCreated**: ein DateTime-Wert, der angibt, wann der DSC-Vorgang begonnen hat.
* **ComputerName**: den Computernamen aus, in dem die Ergebnisse aggregiert werden.
* **Ergebnis**: eine Zeichenfolge mit dem Wert **Fehler** oder **Erfolg** der angibt, ob dieser DSC-Vorgang einen Fehler aufgetreten ist oder nicht, bzw..
* **AllEvents**: ein Objekt, das eine Auflistung von Ereignissen darstellt, die von der DSC-Vorgang erzeugten.

Die folgende Ausgabe zeigt beispielsweise die Ergebnisse des letzten Vorgangs von mehreren Computern:
  TODO: Ersetzen Bild für Get-xDscOperation zum Remotecomputer Protokolle anzeigen

##Trace-xDscOperation

Dieses Cmdlet gibt ein Objekt mit einer Auflistung der Ereignisse, deren Ereignistypen und die Meldungsausgabe aus einem bestimmten DSC-Vorgang generiert. In der Regel bei finden Sie einen Fehler in die Vorgänge mit `Get-xDscOperation`, würden Sie diesen Vorgang, um herauszufinden, welche der Ereignisse ein Fehler verursacht verfolgen.

###Parameter

* **SequenceID**: Dies ist der ganzzahlige Wert auf jeden Vorgang, bezieht sich auf einen bestimmten Computer zugewiesen. Durch Angabe wird eine Sequenz-ID, z. B. 4, Trace für die DSC-Vorgang, der aus der letzten 4. wurde ausgegeben werden.

Trace-xDscOperation mit angegebener Sequenz-ID
* **JobID**: Dies ist der GUID-Wert, der vom LCM xDscOperation zugewiesen werden, um einen Vorgang eindeutig zu identifizieren. Wenn ein JobID angegeben wird, wird die Verfolgung des entsprechenden Vorgangs DSC ausgegeben.
   TODO: Ersetzen Bild für Trace xDscOperation übernimmt die JobID als Parameter
* **ComputerName** und **Anmeldeinformationen**: mit diesen Parametern können Sie die Verfolgung von Remotecomputern gesammelt werden:
```powershell
New-NetFirewallRule -Name "Service RemoteAdmin" -Action Allow
```
  TODO: Ersetzen der Grafik für Trace-xDscOperation auf einen anderen Computer ausführen

Beachten Sie, dass seit `Trace-xDscOperation` Ereignisse aus der analytischen Debug- und Betriebsprotokolle aggregiert, es werden Sie aufgefordert, diese Protokolle zu aktivieren, wie oben beschrieben.

###Zurückgegebene Objekt

Das Cmdlet gibt ein Array von Objekten, jedes Typs `Microsoft.PowerShell.xDscDiagnostics.TraceOutput`. Jedes Objekt in diesem Array enthält die folgenden Felder:
* **ComputerName**: der Name des Computers aus, in dem die Protokolle gesammelt werden.
* **EventType**: Dies ist ein Enumerator-Typ-Feld, das Informationen über den Typ des Ereignisses enthält. Es könnte eines der folgenden sein:
   - *Betrieb*: das Ereignis wird aus dem operativen Protokoll.
   - *Analytische*: das Ereignis wird aus dem analytischen Protokoll.
   - *Debuggen*: das Ereignis wird aus dem Debugprotokoll.
   - *Ausführlich*: Ausgabe als ausführliche Meldungen während der Ausführung von Ereignissen. Ausführlichen Meldungen erleichtern die Abfolge der Ereignisse identifizieren, die veröffentlicht werden.
   - *Fehler*: Fehlerereignisse. Anhand der Fehlerereignisse finden Sie in der Regel schnell den Grund für den Fehler.
* **TimeCreated**: ein DateTime-Wert, der angibt, wann das Ereignis von DSC protokolliert wurde.
* **Nachricht**: die Nachricht, die in den Ereignisprotokollen von DSC protokolliert wurde.

Folgendes sind die Felder in diesem Objekt, die für Weitere Informationen zum Ereignis verwendet werden kann, aber nicht standardmäßig angezeigt:

* **JobID**: die Auftrags-ID (GUID-Format) für diesen Vorgang DSC spezifisch.
* **SequenceID**: die SequenceID für diesen Vorgang DSC auf diesem Computer eindeutig.
* **Ereignis**: Dies ist das tatsächliche Ereignis protokolliert von DSC, vom Typ `System.Diagnostics.Eventing.Reader.EventLogRecord`. Dies kann auch der abgerufenen durch Ausführen des Cmdlets `Get-WinEvent`. Sie enthält weitere Informationen, wie z. B. die Aufgabe, EventID und Ebene des Ereignisses.

Alternativ können Sie Informationen auch auf die Ereignisse sammeln, speichern Sie die Ausgabe des `Trace-xDscOperation` in eine Variable. Den folgenden Befehl können alle Ereignisse für eine bestimmte DSC-Operation anzeigen:

```powershell
(Trace-xDscOperation -SequenceID 3).Event
```

Dadurch wird angezeigt, die gleichen Ergebnisse wie die `Get-WinEvent` Cmdlets, z. B. die folgende Ausgabe:
  TODO: Welche Ausgabe?

Im Idealfall würden Sie zuerst verwenden `Get-xDscOperations` die letzten paar DSC auflisten Konfiguration auf Ihrem Computer ausgeführt wird. Im Anschluss, können Sie mit einem einzelnen Vorgang (mithilfe der SequenceID oder JobID) untersuchen `Trace-xDscOperation` zu ermitteln, was dabei im Hintergrund durchgeführt wurde.

##Meine Ressourcen nicht aktualisiert: Zurücksetzen des Caches

Das Modul DSC speichert Ressourcen, die als PowerShell-Modul aus Effizienzgründen implementiert. Dies kann jedoch Probleme verursachen, wenn Sie eine Ressource erstellen und es gleichzeitig testen, da DSC die zwischengespeicherte Version geladen werden, bis der Prozess neu gestartet wird. Die einzige Möglichkeit, DSC, laden die neuere Version zu machen ist den Prozess, der das Modul DSC hostet explizit zu beenden.

Auf ähnliche Weise beim Ausführen `Start DSCConfiguration`, nach dem Hinzufügen und ändern eine benutzerdefinierte Ressource, die Änderung möglicherweise nicht ausgeführt, es sei denn, oder bis der Computer neu gestartet wird. Dies ist da DSC ausgeführt, in der WMI-Anbieter-Hostprozess (WmiPrvSE wird) und in der Regel stehen viele Instanzen der WmiPrvSE gleichzeitig ausführen. Beim Neustart der Hostprozess wird neu gestartet, und der Cache wird gelöscht.

Um erfolgreich Wiederverwenden der Konfigurations und den Cache zu löschen, ohne neu zu starten, müssen Sie beenden und neu starten des Hostprozesses. Dies kann auf einer Basis pro Instanz erfolgen durch den Prozess zu identifizieren, beenden und starten Sie ihn neu. Alternativ können Sie `Debug-Modus`, wie unten gezeigt, um die PowerShell-DSC Ressource erneut zu laden.

Um zu ermitteln, welcher Prozess die DSC-Engine hostet, und auf einer Basis pro Instanz zu beenden, können Sie die Prozess-ID der WmiPrvSE auflisten, die DSC-Engine hostet. Beenden Sie dann, um den Anbieter zu aktualisieren, der Prozess WmiPrvSE, die mithilfe der unten aufgeführten Befehle ein, und führen Sie **Start DscConfiguration** erneut.

```powershell
###
### find the process that is hosting the DSC engine
###
$dscProcessID = Get-WmiObject msft_providers | 
Where-Object {$_.provider -like 'dsccore'} | 
Select-Object -ExpandProperty HostProcessIdentifier 

###
### Stop the process
###
Get-Process -Id $dscProcessID | Stop-Process
```

##Mithilfe der Debug-Modus

Sie können konfigurieren, dass die DSC lokale Configuration Manager (LCM) verwenden `Debug-Modus` immer den Cache löschen, wenn der Hostprozess neu gestartet wird. Bei Festlegung auf **TRUE**, führt das Modul für die Ressource PowerShell DSC immer neu laden. Sobald Sie fertig sind schreiben Ihre Ressource festlegen, dass an **FALSE** und das Modul wird das Verhalten des Zwischenspeicherns Module zurückgesetzt.

Es folgt eine Demonstration angezeigt wie `Debug-Modus` kann den Cache automatisch aktualisiert. Zunächst sehen wir uns die Standardkonfiguration:

```
PS C:\Users\WinVMAdmin\Desktop> Get-DscLocalConfigurationManager


AllowModuleOverwrite           : False
CertificateID                  : 
ConfigurationID                : 
ConfigurationMode              : ApplyAndMonitor
ConfigurationModeFrequencyMins : 30
Credential                     : 
DebugMode                      : False
DownloadManagerCustomData      : 
DownloadManagerName            : 
LocalConfigurationManagerState : Ready
RebootNodeIfNeeded             : False
RefreshFrequencyMins           : 15
RefreshMode                    : PUSH
PSComputerName                 :  
```

Sie können sehen, `Debug-Modus` minFreeThreads auf **FALSE**.

Einrichten der `Debug-Modus` Demo verwenden Sie die folgende PowerShell-Ressource:

```powershell
function Get-TargetResource
{
    param
    (
        [Parameter(Mandatory)]
        $onlyProperty
    )
    return @{onlyProperty = Get-Content -path "$env:SystemDrive\OutputFromTestProviderDebugMode.txt"}
}
function Set-TargetResource
{
    param
    (
        [Parameter(Mandatory)]
        $onlyProperty
    )
    "1"|Out-File -PSPath "$env:SystemDrive\OutputFromTestProviderDebugMode.txt"
}
function Test-TargetResource
{
    param
    (
        [Parameter(Mandatory)]
        $onlyProperty
    )
    return $false
} 
```

Erstellen Sie jetzt eine Konfiguration mit den oben genannten Ressource mit dem Namen `TestProviderDebugMode`:

```powershell
Configuration ConfigTestDebugMode
{
    Import-DscResource -Name TestProviderDebugMode
    Node localhost
    {
        TestProviderDebugMode test
        {
            onlyProperty = "blah"
        }
    }
}
ConfigTestDebugMode
```

Sehen Sie, dass der Inhalt der Datei: "**$env:SystemDrive\OutputFromTestProviderDebugMode.txt**" ist **1**.

Aktualisieren Sie jetzt den Anbietercode mithilfe des folgenden Skripts:

```powershell
$newResourceOutput = Get-Random -Minimum 5 -Maximum 30
$content = @"
function Get-TargetResource
{
    param
    (
        [Parameter(Mandatory)]
        `$onlyProperty
    )
    return @{onlyProperty = Get-Content -path "$env:SystemDrive\OutputFromTestProviderDebugMode.txt"}
}
function Set-TargetResource
{
    param
    (
        [Parameter(Mandatory)]
        `$onlyProperty
    )
    "$newResourceOutput"|Out-File -PSPath C:\OutputFromTestProviderDebugMode.txt
}
function Test-TargetResource
{
    param
    (
        [Parameter(Mandatory)]
        `$onlyProperty
    )
    return `$false
}
"@ | Out-File -FilePath "C:\Program Files\WindowsPowerShell\Modules\MyPowerShellModules\DSCResources\TestProviderDebugMode\TestProviderDebugMode.psm1
```

Dieses Skript wird eine Zufallszahl generiert und den Anbieter cade entsprechend aktualisiert. Mit `Debug-Modus` auf False gesetzt, den Inhalt der Datei "**$env:SystemDrive\OutputFromTestProviderDebugMode.txt**" niemals geändert werden.

Legen Sie jetzt `Debug-Modus` auf **TRUE** in Ihrem Skript:

```powershell
LocalConfigurationManager
{
    DebugMode = $true
} 
```

Wenn Sie das obige Skript erneut ausführen, sehen Sie, dass der Inhalt der Datei jedes Mal anders ist. (Sie können ausführen `Get-DscConfiguration` überprüfen). Im folgenden ist das Ergebnis von zwei zusätzliche ausgeführt wird (die Ergebnisse möglicherweise anders, wenn Sie das Skript ausführen):

```powershell
PS C:\Users\WinVMAdmin\Desktop> Get-DscConfiguration -CimSession (New-CimSession localhost)

onlyProperty                            PSComputerName                         
------------                            --------------                         
20                                      localhost                              

PS C:\Users\WinVMAdmin\Desktop> Get-DscConfiguration -CimSession (New-CimSession localhost)

onlyProperty                            PSComputerName                         
------------                            --------------                         
14 
```

##Weitere Informationen

###Referenz

* [DSC-Protokoll-Ressource](logResource.md)

###Konzepte

* [Erstellen Sie benutzerdefinierter Windows PowerShell gewünscht State Configuration-Ressourcen](authoringResource.md)

###Weitere Ressourcen

* [WindowsPowerShell gewünschten Status Konfigurations-Cmdlets] (https://technet.microsoft.com/en-us/library/dn521624 (v=wps.630).aspx)




