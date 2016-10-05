---
title: Problembehandlung bei DSC
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 4830be14b105485c50446f06e9d36491b4c4fe44

---

# Problembehandlung bei DSC

>Gilt für: Windows PowerShell 4.0, Windows PowerShell 5.0

Dieses Thema beschreibt die Problembehandlung für DSC.

## Verwenden von „Get-DscConfigurationStatus“

Das Cmdlet [Get-DscConfigurationStatus](https://technet.microsoft.com/en-us/library/mt517868.aspx) ruft Informationen zum Konfigurationsstatus von einem Zielknoten ab. Ein umfangreiches Objekt wird zurückgegeben, das ausführliche Informationen dazu enthält, ob die Ausführung der Konfiguration erfolgreich war oder nicht. Sie können das Objekt eingehender untersuchen, um Details zur Ausführung der Konfiguration zu ermitteln, wie z. B.:

* Alle fehlerhaften Ressourcen
* Alle Ressourcen, die ein Neustart erfordern
* Metakonfigurationseinstellungen zum Zeitpunkt der Konfigurationsausführung
* usw.

Die folgende Parametergruppe gibt die Statusinformationen zur letzten Konfigurationsausführung zurück:

```powershell
Get-DscConfigurationStatus  [-CimSession <CimSession[]>] 
                            [-ThrottleLimit <int>] 
                            [-AsJob] 
                            [<CommonParameters>]
```
Die folgende Parametergruppe gibt die Statusinformationen zu allen vorherigen Konfigurationsausführungen zurück:

```powershell
Get-DscConfigurationStatus  -All 
                            [-CimSession <CimSession[]>] 
                            [-ThrottleLimit <int>] 
                            [-AsJob] 
                            [<CommonParameters>]
```

## Beispiel

```powershell
PS C:\> $Status = Get-DscConfigurationStatus 

PS C:\> $Status

Status      StartDate               Type            Mode    RebootRequested     NumberOfResources
------      ---------               ----            ----    ---------------     -----------------
Failure     11/24/2015  3:44:56     Consistency     Push    True                36

PS C:\> $Status.ResourcesNotInDesiredState

ConfigurationName       :   MyService
DependsOn               :   
ModuleName              :   PSDesiredStateConfiguration
ModuleVersion           :   1.1
PsDscRunAsCredential    :   
ResourceID              :   [File]ServiceDll
SourceInfo              :   c:\git\CustomerService\Configs\MyCustomService.ps1::5::34::File
DurationInSeconds       :   0.19
Error                   :   SourcePath must be accessible for current configuration. The related file/directory is:
                            \\Server93\Shared\contosoApp.dll. The related ResourceID is [File]ServiceDll
FinalState              :   
InDesiredState          :   False
InitialState            :   
InstanceName            :   ServiceDll
RebootRequested         :   False
ReosurceName            :   File
StartDate               :   11/24/2015  3:44:56
PSComputerName          :
```

## Mein Skript wird nicht ausgeführt: Verwenden von DSC-Protokollen für die Diagnose von Skriptfehlern

Wie alle Windows-Softwareprogramme erfasst DSC Fehler und Ereignisse in [Protokollen](https://msdn.microsoft.com/library/windows/desktop/aa363632.aspx), die Sie in der [Ereignisanzeige](http://windows.microsoft.com/windows/what-information-event-logs-event-viewer) anzeigen können. Die Durchsicht dieser Protokolle kann Ihnen dabei helfen herauszufinden, warum ein bestimmter Vorgang fehlgeschlagen ist und wie Sie Fehler in Zukunft vermeiden. Das Schreiben von Konfigurationsskripts ist nicht ganz einfach. Sie sollten daher den Fortschritt der Konfiguration im Ereignisprotokoll der DSC-Analyse mithilfe der DSC-Protokollressource verfolgen, um Fehler bei der Erstellung leichter aufspüren zu können.

## Wo befinden sich die DSC-Ereignisprotokolle?

In der Ereignisanzeige werden DSC-Ereignisse unter **Anwendungs- und Dienstprotokolle/Microsoft/Windows/Desired State Configuration** angezeigt.

Die können auch das entsprechende PowerShell-Cmdlet [Get-WinEvent](https://technet.microsoft.com/library/hh849682.aspx) ausführen, um die Ereignisprotokolle anzuzeigen:

```
PS C:\> Get-WinEvent -LogName "Microsoft-Windows-Dsc/Operational"
   ProviderName: Microsoft-Windows-DSC
TimeCreated                     Id LevelDisplayName Message                                                                                                  
-----------                     -- ---------------- -------                                                                                                  
11/17/2014 10:27:23 PM        4102 Information      Job {02C38626-D95A-47F1-9DA2-C1D44A7128E7} : 
```

Wie oben gezeigt, lautet der primäre Protokollname von DSC **Microsoft -> Windows -> DSC** (andere Protokollnamen unter Windows werden hier aus Gründen der Übersichtlichkeit nicht dargestellt). Der primäre Name wird an den Namen des Kanals angefügt. Daraus ergibt sich der vollständige Protokollname. Das DSC-Modul schreibt hauptsächlich in drei Protokolltypen: [Betriebsprotokoll, analytisches Protokoll und Debugprotokoll](https://technet.microsoft.com/library/cc722404.aspx). Da die analytischen Protokolle und die Debugprotokolle standardmäßig deaktiviert sind, sollten Sie sie in der Ereignisanzeige aktivieren. Öffnen Sie dazu die Ereignisanzeige, indem Sie in Windows PowerShell „Show-EventLog“ eingeben. Oder klicken Sie auf die Schaltfläche **Start** und dann auf **Systemsteuerung**, **Verwaltung** und **Ereignisanzeige**. Klicken Sie in der Ereignisanzeige im Menü **Ansicht** auf **Analytische und Debugprotokolle einblenden**. Der Name des für den analytischen Kanal lautet **Microsoft-Windows-Dsc/Analytic**, und der Debugkanal heißt **Microsoft-Windows-Dsc/Debug**. Außerdem können Sie zum Aktivieren der Protokolle das Hilfsprogramm [wevtutil](https://technet.microsoft.com/library/cc732848.aspx) verwenden, wie im folgenden Beispiel gezeigt.

```powershell
wevtutil.exe set-log “Microsoft-Windows-Dsc/Analytic” /q:true /e:true
```

## Was ist in den DSC-Protokollen enthalten?

DSC-Protokolle werden, basierend auf der Wichtigkeit der Meldung, auf die drei Protokollkanäle verteilt. Das Betriebsprotokoll in DSC enthält alle Fehlermeldungen und kann verwendet werden, um ein Problem zu identifizieren. Das analytische Protokoll enthält eine größere Anzahl von Ereignissen und dient dazu zu ermitteln, wo ein Fehler aufgetreten ist. Dieser Kanal enthält auch ausführliche Meldungen (falls vorhanden). Das Debugprotokoll enthält Protokolle, die Ihnen helfen zu verstehen, wie der Fehler entstanden ist. DSC-Ereignismeldungen sind so aufgebaut, dass jede Ereignismeldung mit eine Auftrags-ID beginnt, die einen DSC-Vorgang eindeutig identifiziert. Im folgenden Beispiel wird versucht, die Meldung über das erste Ereignis abzurufen, das im DSC-Betriebsprotokoll protokolliert wurde.

```powershell
PS C:\> $AllDscOpEvents = Get-WinEvent -LogName "Microsoft-Windows-Dsc/Operational"
PS C:\> $FirstOperationalEvent = $AllDscOpEvents[0]
PS C:\> $FirstOperationalEvent.Message
Job {02C38626-D95A-47F1-9DA2-C1D44A7128E7} : 
Consistency engine was run successfully. 
```

DSC-Ereignisse werden in einer bestimmten Struktur protokolliert, das dem Benutzer ermöglicht, Ereignisse eines DSC-Auftrags zusammenzufassen. Die Struktur sieht wie folgt aus:

**Auftrags-ID: <Guid>**
**<Event Message>**

## Sammeln von Ereignissen zu einem einzelnen DSC-Vorgang

DSC-Ereignisprotokolle enthalten Ereignisse, die mithilfe verschiedener DSC-Vorgänge generiert wurden. Allerdings interessieren Sie sich in der Regel nur für die Details zu einem bestimmten Vorgang. Alle DSC-Protokolle können nach der Auftrags-ID-Eigenschaft gruppiert werden, die für jeden DSC-Vorgang eindeutig ist. Die Auftrags-ID wird in allen DSC-Ereignissen als erster Eigenschaftswert angezeigt. Die folgenden Schritte erläutern, wie Sie alle Ereignisse in einer gruppierten Arraystruktur sammeln.

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
 
$DscEvents=[System.Array](Get-WinEvent "Microsoft-Windows-Dsc/Operational") `
         + [System.Array](Get-WinEvent "Microsoft-Windows-Dsc/Analytic" -Oldest) `
         + [System.Array](Get-WinEvent "Microsoft-Windows-Dsc/Debug" -Oldest)
 
 
<##########################################################################
 Step 4 : Group all logs based on the job ID
###########################################################################>
$SeparateDscOperations = $DscEvents | Group {$_.Properties[0].value}  
```

Hier enthält die Variable `$SeparateDscOperations` nach den Auftrags-IDs gruppierte Protokolle. Jedes Arrayelement dieser Variablen stellt eine Gruppe von Ereignissen dar, die von einem anderen DSC-Vorgang protokolliert wurden, und ermöglicht den Zugriff auf weitere Informationen zu den Protokollen .

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

Sie können die Daten in der Variablen `$SeparateDscOperations` mit [Where-Object](https://technet.microsoft.com/library/ee177028.aspx) extrahieren. In den folgenden fünf Szenarien ist es beispielsweise sinnvoll, Daten für die Problembehandlung bei DSC zu extrahieren:

### 1: Fehler bei Vorgängen

Alle Ereignisse verfügen über [Schweregrade](https://msdn.microsoft.com/library/dd996917(v=vs.85)). Diese Informationen kann verwendet werden, um die Fehlerereignisse zu identifizieren:

```
PS C:\> $SeparateDscOperations | Where-Object {$_.Group.LevelDisplayName -contains "Error"}
Count Name                      Group                                                                     
----- ----                      -----                                                                     
   38 {5BCA8BE7-5BB6-11E3-BF... {System.Diagnostics.Eventing.Reader.EventLogRecord, System.Diagnostics....
```

### 2: Details zu Vorgängen, die in der letzten halben Stunde ausgeführt wurden

`TimeCreated`, eine Eigenschaft jedes Windows-Ereignisses, gibt den Zeitpunkt an, zu dem das Ereignis erstellt wurde. Durch Vergleichen dieser Eigenschaft mit einem bestimmten Datum/Uhrzeit-Objekt können Sie alle Ereignisse filtern:

```powershell
PS C:\> $DateLatest = (Get-Date).AddMinutes(-30)
PS C:\> $SeparateDscOperations | Where-Object {$_.Group.TimeCreated -gt $DateLatest}
Count Name                      Group                                                                     
----- ----                      -----                                                                     
    1 {6CEC5B09-5BB0-11E3-BF... {System.Diagnostics.Eventing.Reader.EventLogRecord}   
```

### 3: Meldungen zum aktuellen Vorgang

Der aktuelle Vorgang wird im ersten Index der Arraygruppe `$SeparateDscOperations` gespeichert. Durch Abfragen der Meldungen der Gruppe für den Index 0 werden alle Meldungen für den aktuellen Vorgang zurückgegeben:

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

### 4: Fehlermeldungen, die für die letzten fehlgeschlagenen Vorgänge protokolliert wurden

`$SeparateDscOperations[0].Group` enthält eine Reihe von Ereignissen für den aktuellen Vorgang. Führen Sie das Cmdlet `Where-Object` aus, um die Ereignisse basierend auf dem Schweregrad/Anzeigenamen zu filtern. Die Ergebnisse werden in der Variablen `$myFailedEvent` gespeichert. Diese kann weiter zerlegt werden, um die Ereignismeldung zu erhalten:

```powershell
PS C:\> $myFailedEvent = ($SeparateDscOperations[0].Group | Where-Object {$_.LevelDisplayName -eq "Error"})
 
PS C:\> $myFailedEvent.Message
Job {5BCA8BE7-5BB6-11E3-BF41-00155D553612} : 
DSC Engine Error : 
 Error Message Current configuration does not exist. Execute Start-DscConfiguration command with -Path pa
rameter to specify a configuration file and create a current configuration first. 
Error Code : 1 
```

### 5: Alle Ereignisse, die für eine bestimmte Auftrags-ID generiert wurden.

`$SeparateDscOperations` ist ein Array von Gruppen, die jeweils den Namen als eindeutige Auftrags-ID aufweisen. Durch Ausführen des Cmdlets `Where-Object` können Sie die Gruppen von Ereignissen mit einer bestimmten Auftrags-ID extrahieren:

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

## Verwenden von „xDscDiagnostics“ zum Analysieren von DSC-Protokollen

**xDscDiagnostics** ist ein PowerShell-Modul, das aus mehreren Funktionen besteht, die bei der Analyse von DSC-Fehlern auf dem Computer helfen können. Diese Funktionen können Ihnen helfen, alle lokalen Ereignisse der letzten DSC-Vorgänge oder DSC-Ereignisse auf Remotecomputern (mit gültigen Anmeldeinformationen) zu identifizieren. Hier wird der Begriff „DSC-Vorgang“ verwendet, um eine einzelne eindeutige DSC-Ausführung von Anfang bis Ende zu definieren. `Test-DscConfiguration` wäre z. B. ein separater DSC-Vorgang. Auf ähnliche Weise kann jedes andere Cmdlet in DSC (z. B. `Get-DscConfiguration`, `Start-DscConfiguration` usw.) jeweils als separater DSC-Vorgang identifiziert werden. Die Funktionen werden unter [xDscDiagnostics](https://github.com/PowerShell/xDscDiagnostics) erläutert. Hilfe ist durch Ausführen von `Get-Help <cmdlet name>` verfügbar.

### Abrufen von Details der DSC-Vorgänge 

Mit der Funktion `Get-xDscOperation` können Sie die Ergebnisse der DSC-Vorgänge suchen, die auf einem oder mehreren Computern ausgeführt werden. Die Funktion gibt außerdem ein Objekt zurück, das die Sammlung der von den einzelnen DSC-Vorgängen erzeugten Ereignisse enthält. In der folgenden Ausgabe wurden beispielsweise drei Befehle ausgeführt. Der erste wurde erfolgreich ausgeführt, bei den beiden anderen sind Fehler aufgetreten. Die Ergebnisse sind in der Ausgabe von `Get-xDscOperation` zusammengefasst.

```powershell
PS C:\DiagnosticsTest> Get-xDscOperation

ComputerName   SequenceId TimeCreated           Result   JobID                                 AllEvents            
------------   ---------- -----------           ------   -----                                 ---------            
SRV1   1          6/23/2016 9:37:52 AM  Failure  9701aadf-395e-11e6-9165-00155d390509  {@{Message=; TimeC...
SRV1   2          6/23/2016 9:36:54 AM  Failure  7e8e2d6e-395c-11e6-9165-00155d390509  {@{Message=; TimeC...
SRV1   3          6/23/2016 9:36:54 AM  Success  af72c6aa-3960-11e6-9165-00155d390509  {@{Message=Operati...

```

Durch Verwendung des Parameters `Newest` können Sie angeben, dass nur die Ergebnisse der aktuellsten Vorgänge ausgegeben werden sollen:

```powershell
PS C:\DiagnosticsTest> Get-xDscOperation -Newest 5
ComputerName   SequenceId TimeCreated           Result   JobID                                 AllEvents            
------------   ---------- -----------           ------   -----                                 ---------            
SRV1   1          6/23/2016 4:36:54 PM  Success                                        {@{Message=; TimeC...
SRV1   2          6/23/2016 4:36:54 PM  Success  5c06402b-399b-11e6-9165-00155d390509  {@{Message=Operati...
SRV1   3          6/23/2016 4:36:54 PM  Success                                        {@{Message=; TimeC...
SRV1   4          6/23/2016 4:36:54 PM  Success  5c06402a-399b-11e6-9165-00155d390509  {@{Message=Operati...
SRV1   5          6/23/2016 4:36:51 PM  Success                                        {@{Message=; TimeC...
```

### Abrufen von Details der DSC-Ereignisse

Das Cmdlet „Trace-xDscOperation1“ gibt ein Objekt zurück, das eine Sammlung von Ereignissen, deren Ereignistypen und die von einem bestimmten DSC-Vorgang generierte Meldungsausgabe enthält. Wenn bei einem der Get-xDscOperation-Vorgänge ein Fehler auftritt, sollten Sie den Vorgang nachverfolgen, um herauszufinden, welches der Ereignisse den Fehler hervorgerufen hat.

Verwenden Sie den Parameter `SequenceID`, um die Ereignisse eines bestimmten Vorgangs auf einem bestimmten Computer abzurufen. Wenn Sie beispielsweise für `SequenceID` „9“ angeben, ruft `Trace-xDscOperaion` die Verfolgung des neuntletzten DSC-Vorgangs ab:

```powershell
PS C:\DiagnosticsTest> Trace-xDscOperation -SequenceID 9

ComputerName   EventType    TimeCreated           Message                                                                                             
------------   ---------    -----------           -------                                                                                             
SRV1   OPERATIONAL  6/24/2016 10:51:52 AM Operation Consistency Check or Pull started by user sid S-1-5-20 from computer NULL.                
SRV1   OPERATIONAL  6/24/2016 10:51:52 AM Running consistency engine.                                                                         
SRV1   OPERATIONAL  6/24/2016 10:51:52 AM The local configuration manager is updating the PSModulePath to WindowsPowerShell\Modules;C:\Prog...
SRV1   OPERATIONAL  6/24/2016 10:51:53 AM  Resource execution sequence :: [WindowsFeature]DSCServiceFeature, [xDSCWebService]PSDSCPullServer. 
SRV1   OPERATIONAL  6/24/2016 10:51:54 AM Consistency engine was run successfully.                                                            
SRV1   OPERATIONAL  6/24/2016 10:51:54 AM Job runs under the following LCM setting. ...                                                       
SRV1   OPERATIONAL  6/24/2016 10:51:54 AM Operation Consistency Check or Pull completed successfully. 
```

Übergeben Sie die einem bestimmten DSC-Vorgang zugeordnete **GUID** (wie vom Cmdlet `Get-xDscOperation` zurückgegeben), um die Ereignisdetails für diesen DSC-Vorgang abzurufen:

```powershell
PS C:\DiagnosticsTest> Trace-xDscOperation -JobID 9e0bfb6b-3a3a-11e6-9165-00155d390509

ComputerName   EventType    TimeCreated           Message                                                                                             
------------   ---------    -----------           -------                                                                                             
SRV1   OPERATIONAL  6/24/2016 11:36:56 AM Operation Consistency Check or Pull started by user sid S-1-5-20 from computer NULL.                
SRV1   ANALYTIC     6/24/2016 11:36:56 AM Deleting file from C:\Windows\System32\Configuration\DSCEngineCache.mof                             
SRV1   OPERATIONAL  6/24/2016 11:36:56 AM Running consistency engine.                                                                         
SRV1   VERBOSE      6/24/2016 11:36:56 AM [SRV1]:                            [] Starting consistency engine.                          
SRV1   ANALYTIC     6/24/2016 11:36:56 AM Applying configuration from C:\Windows\System32\Configuration\Current.mof.                          
SRV1   ANALYTIC     6/24/2016 11:36:56 AM Parsing the configuration to apply.                                                                 
SRV1   OPERATIONAL  6/24/2016 11:36:56 AM  Resource execution sequence :: [WindowsFeature]DSCServiceFeature, [xDSCWebService]PSDSCPullServer. 
SRV1   VERBOSE      6/24/2016 11:36:56 AM [SRV1]: LCM:  [ Start  Resource ]  [[WindowsFeature]DSCServiceFeature]                      
SRV1   ANALYTIC     6/24/2016 11:36:56 AM Executing operations for PS DSC resource MSFT_RoleResource with resource name [WindowsFeature]DSC...
SRV1   VERBOSE      6/24/2016 11:36:56 AM [SRV1]: LCM:  [ Start  Test     ]  [[WindowsFeature]DSCServiceFeature]                      
SRV1   VERBOSE      6/24/2016 11:36:56 AM [SRV1]:                            [[WindowsFeature]DSCServiceFeature] The operation 'Get...
SRV1   VERBOSE      6/24/2016 11:36:56 AM [SRV1]:                            [[WindowsFeature]DSCServiceFeature] The operation 'Get...
SRV1   VERBOSE      6/24/2016 11:36:56 AM [SRV1]: LCM:  [ End    Test     ]  [[WindowsFeature]DSCServiceFeature] True in 0.3130 sec...
SRV1   VERBOSE      6/24/2016 11:36:56 AM [SRV1]: LCM:  [ End    Resource ]  [[WindowsFeature]DSCServiceFeature]                      
SRV1   VERBOSE      6/24/2016 11:36:56 AM [SRV1]: LCM:  [ Start  Resource ]  [[xDSCWebService]PSDSCPullServer]                        
SRV1   ANALYTIC     6/24/2016 11:36:56 AM Executing operations for PS DSC resource MSFT_xDSCWebService with resource name [xDSCWebService]P...
SRV1   VERBOSE      6/24/2016 11:36:56 AM [SRV1]: LCM:  [ Start  Test     ]  [[xDSCWebService]PSDSCPullServer]                        
SRV1   VERBOSE      6/24/2016 11:36:56 AM [SRV1]:                            [[xDSCWebService]PSDSCPullServer] Check Ensure           
SRV1   VERBOSE      6/24/2016 11:36:56 AM [SRV1]:                            [[xDSCWebService]PSDSCPullServer] Check Port             
SRV1   VERBOSE      6/24/2016 11:36:56 AM [SRV1]:                            [[xDSCWebService]PSDSCPullServer] Check Physical Path ...
SRV1   VERBOSE      6/24/2016 11:36:56 AM [SRV1]:                            [[xDSCWebService]PSDSCPullServer] Check State            
SRV1   VERBOSE      6/24/2016 11:36:56 AM [SRV1]:                            [[xDSCWebService]PSDSCPullServer] Get Full Path for We...
SRV1   VERBOSE      6/24/2016 11:36:56 AM [SRV1]: LCM:  [ End    Test     ]  [[xDSCWebService]PSDSCPullServer] True in 0.0160 seconds.
SRV1   VERBOSE      6/24/2016 11:36:56 AM [SRV1]: LCM:  [ End    Resource ]  [[xDSCWebService]PSDSCPullServer]                        
SRV1   VERBOSE      6/24/2016 11:36:56 AM [SRV1]:                            [] Consistency check completed.                          
SRV1   ANALYTIC     6/24/2016 11:36:56 AM Deleting file from C:\Windows\System32\Configuration\DSCEngineCache.mof                             
SRV1   OPERATIONAL  6/24/2016 11:36:56 AM Consistency engine was run successfully.                                                            
SRV1   OPERATIONAL  6/24/2016 11:36:56 AM Job runs under the following LCM setting. ...                                                       
SRV1   OPERATIONAL  6/24/2016 11:36:56 AM Operation Consistency Check or Pull completed successfully.                                         
SRV1   ANALYTIC     6/24/2016 11:36:56 AM Deleting file from C:\Windows\System32\Configuration\DSCEngineCache.mof
```

Beachten Sie Folgendes: Da `Trace-xDscOperation` Ereignisse aus den analytischen Protokollen, den Debugprotokollen und den Betriebsprotokollen aggregiert, werden Sie aufgefordert, diese Protokolle wie oben beschrieben zu aktivieren.

Informationen zu den Ereignissen können Sie alternativ auch sammeln, indem Sie die Ausgabe von `Trace-xDscOperation` in einer Variablen speichern. Verwenden Sie die folgenden Befehle, um alle Ereignisse für einen bestimmten DSC-Vorgang anzuzeigen.

```powershell
PS C:\DiagnosticsTest> $Trace = Trace-xDscOperation -SequenceID 4

PS C:\DiagnosticsTest> $Trace.Event
```

Dadurch werden gleichen Ergebnisse wie durch das Cmdlet `Get-WinEvent` in der unten stehenden Ausgabe angezeigt:

```powershell
   ProviderName: Microsoft-Windows-DSC

TimeCreated                     Id LevelDisplayName Message                                                                                           
-----------                     -- ---------------- -------                                                                                           
6/23/2016 1:36:53 AM          4312 Information      The DscTimer is running LCM method PerformRequiredConfigurationChecks with the flag set to 5.     
6/23/2016 1:36:53 AM          4343 Information      The DscTimer has successfully run LCM method PerformRequiredConfigurationChecks with flag 5.      
6/23/2016 2:07:00 AM          4312 Information      The DscTimer is running LCM method PerformRequiredConfigurationChecks with the flag set to 5.     
6/23/2016 2:07:01 AM          4343 Information      The DscTimer has successfully run LCM method PerformRequiredConfigurationChecks with flag 5.      
6/23/2016 2:36:55 AM          4312 Information      The DscTimer is running LCM method PerformRequiredConfigurationChecks with the flag set to 5.     
6/23/2016 2:36:56 AM          4343 Information      The DscTimer has successfully run LCM method PerformRequiredConfigurationChecks with flag 5.      
6/23/2016 3:06:55 AM          4312 Information      The DscTimer is running LCM method PerformRequiredConfigurationChecks with the flag set to 5.     
6/23/2016 3:06:55 AM          4343 Information      The DscTimer has successfully run LCM method PerformRequiredConfigurationChecks with flag 5.      
6/23/2016 3:36:55 AM          4312 Information      The DscTimer is running LCM method PerformRequiredConfigurationChecks with the flag set to 5.     
6/23/2016 3:36:55 AM          4343 Information      The DscTimer has successfully run LCM method PerformRequiredConfigurationChecks with flag 5.      
6/23/2016 4:06:53 AM          4312 Information      The DscTimer is running LCM method PerformRequiredConfigurationChecks with the flag set to 5.     
6/23/2016 4:06:53 AM          4343 Information      The DscTimer has successfully run LCM method PerformRequiredConfigurationChecks with flag 5.      
6/23/2016 4:36:52 AM          4312 Information      The DscTimer is running LCM method PerformRequiredConfigurationChecks with the flag set to 5.     
6/23/2016 4:36:53 AM          4343 Information      The DscTimer has successfully run LCM method PerformRequiredConfigurationChecks with flag 5.      
6/23/2016 5:06:52 AM          4312 Information      The DscTimer is running LCM method PerformRequiredConfigurationChecks with the flag set to 5.     
6/23/2016 5:06:53 AM          4343 Information      The DscTimer has successfully run LCM method PerformRequiredConfigurationChecks with flag 5.      
6/23/2016 5:36:54 AM          4312 Information      The DscTimer is running LCM method PerformRequiredConfigurationChecks with the flag set to 5.     
6/23/2016 5:36:54 AM          4343 Information      The DscTimer has successfully run LCM method PerformRequiredConfigurationChecks with flag 5.      
6/23/2016 6:06:52 AM          4312 Information      The DscTimer is running LCM method PerformRequiredConfigurationChecks with the flag set to 5.     
6/23/2016 6:06:53 AM          4343 Information      The DscTimer has successfully run LCM method PerformRequiredConfigurationChecks with flag 5.      
6/23/2016 6:36:56 AM          4312 Information      The DscTimer is running LCM method PerformRequiredConfigurationChecks with the flag set to 5.     
6/23/2016 6:36:57 AM          4343 Information      The DscTimer has successfully run LCM method PerformRequiredConfigurationChecks with flag 5.      
6/23/2016 7:06:52 AM          4312 Information      The DscTimer is running LCM method PerformRequiredConfigurationChecks with the flag set to 5.     
6/23/2016 7:06:53 AM          4343 Information      The DscTimer has successfully run LCM method PerformRequiredConfigurationChecks with flag 5.      
6/23/2016 7:36:53 AM          4312 Information      The DscTimer is running LCM method PerformRequiredConfigurationChecks with the flag set to 5.     
6/23/2016 7:36:54 AM          4343 Information      The DscTimer has successfully run LCM method PerformRequiredConfigurationChecks with flag 5.      
6/23/2016 8:06:54 AM          4312 Information      The DscTimer is running LCM method PerformRequiredConfigurationChecks with the flag set to 5.
```

Im Idealfall würden Sie zuerst `Get-xDscOperation` verwenden, um die letzten DSC-Konfigurationsausführungen auf Ihren Computern aufzulisten. Im Anschluss können Sie jeden einzelnen Vorgang (anhand der Sequenz-ID oder der Auftrags-ID) mit `Trace-xDscOperation` untersuchen, um zu ermitteln, was im Hintergrund geschehen ist.

### Abrufen von Ereignissen für einen Remotecomputer

Verwenden Sie den Parameter `ComputerName` des Cmdlets `Trace-xDscOperation`, um Details von Ereignissen abzurufen, die auf einem Remotecomputer auftreten. Zuvor müssen Sie eine Firewallregel erstellen, um die Remoteverwaltung auf dem Remotecomputer zu erlauben:

```powershell
New-NetFirewallRule -Name "Service RemoteAdmin" -DisplayName "Remote" -Action Allow
```
Nun können Sie diesen Computer angeben, wenn Sie `Trace-xDscOperation` aufrufen:

```powershell
PS C:\DiagnosticsTest> Trace-xDscOperation -ComputerName SRV2 -Credential Get-Credential -SequenceID 5

ComputerName   EventType    TimeCreated           Message
------------   ---------    -----------           -------
SRV2   OPERATIONAL  6/24/2016 11:36:56 AM Operation Consistency Check or Pull started by user sid S-1-5-20 f...
SRV2   ANALYTIC     6/24/2016 11:36:56 AM Deleting file from C:\Windows\System32\Configuration\DSCEngineCach...
SRV2   OPERATIONAL  6/24/2016 11:36:56 AM Running consistency engine.
SRV2   VERBOSE      6/24/2016 11:36:56 AM [SRV2]:                            [] Starting consistency...
SRV2   ANALYTIC     6/24/2016 11:36:56 AM Applying configuration from C:\Windows\System32\Configuration\Curr...
SRV2   ANALYTIC     6/24/2016 11:36:56 AM Parsing the configuration to apply.
SRV2   OPERATIONAL  6/24/2016 11:36:56 AM  Resource execution sequence :: [WindowsFeature]DSCServiceFeature,...
SRV2   VERBOSE      6/24/2016 11:36:56 AM [SRV2]: LCM:  [ Start  Resource ]  [[WindowsFeature]DSCSer...
SRV2   ANALYTIC     6/24/2016 11:36:56 AM Executing operations for PS DSC resource MSFT_RoleResource with re...
SRV2   VERBOSE      6/24/2016 11:36:56 AM [SRV2]: LCM:  [ Start  Test     ]  [[WindowsFeature]DSCSer...
SRV2   VERBOSE      6/24/2016 11:36:56 AM [SRV2]:                            [[WindowsFeature]DSCSer...
SRV2   VERBOSE      6/24/2016 11:36:56 AM [SRV2]:                            [[WindowsFeature]DSCSer...
SRV2   VERBOSE      6/24/2016 11:36:56 AM [SRV2]: LCM:  [ End    Test     ]  [[WindowsFeature]DSCSer...
SRV2   VERBOSE      6/24/2016 11:36:56 AM [SRV2]: LCM:  [ End    Resource ]  [[WindowsFeature]DSCSer...
SRV2   VERBOSE      6/24/2016 11:36:56 AM [SRV2]: LCM:  [ Start  Resource ]  [[xDSCWebService]PSDSCP...
SRV2   ANALYTIC     6/24/2016 11:36:56 AM Executing operations for PS DSC resource MSFT_xDSCWebService with ...
SRV2   VERBOSE      6/24/2016 11:36:56 AM [SRV2]: LCM:  [ Start  Test     ]  [[xDSCWebService]PSDSCP...
SRV2   VERBOSE      6/24/2016 11:36:56 AM [SRV2]:                            [[xDSCWebService]PSDSCP...
SRV2   VERBOSE      6/24/2016 11:36:56 AM [SRV2]:                            [[xDSCWebService]PSDSCP...
SRV2   VERBOSE      6/24/2016 11:36:56 AM [SRV2]:                            [[xDSCWebService]PSDSCP...
SRV2   VERBOSE      6/24/2016 11:36:56 AM [SRV2]:                            [[xDSCWebService]PSDSCP...
SRV2   VERBOSE      6/24/2016 11:36:56 AM [SRV2]:                            [[xDSCWebService]PSDSCP...
SRV2   VERBOSE      6/24/2016 11:36:56 AM [SRV2]: LCM:  [ End    Test     ]  [[xDSCWebService]PSDSCP...
SRV2   VERBOSE      6/24/2016 11:36:56 AM [SRV2]: LCM:  [ End    Resource ]  [[xDSCWebService]PSDSCP...
SRV2   VERBOSE      6/24/2016 11:36:56 AM [SRV2]:                            [] Consistency check co...
SRV2   ANALYTIC     6/24/2016 11:36:56 AM Deleting file from C:\Windows\System32\Configuration\DSCEngineCach...
SRV2   OPERATIONAL  6/24/2016 11:36:56 AM Consistency engine was run successfully.
SRV2   OPERATIONAL  6/24/2016 11:36:56 AM Job runs under the following LCM setting. ...
SRV2   OPERATIONAL  6/24/2016 11:36:56 AM Operation Consistency Check or Pull completed successfully.
SRV2   ANALYTIC     6/24/2016 11:36:56 AM Deleting file from C:\Windows\System32\Configuration\DSCEngineCach...
```

## Meine Ressourcen werden nicht aktualisiert: Zurücksetzen des Caches

Das DSC-Modul speichert Ressourcen zwischen, die aus Effizienzgründen als PowerShell-Modul implementiert wurden. Dies kann jedoch Probleme verursachen, wenn Sie eine Ressource erstellen und gleichzeitig testen, da DSC die zwischengespeicherte Version lädt, solange der Vorgang nicht neu gestartet wurde. Die einzige Möglichkeit, DSC zu veranlassen, die neuere Version zu laden, besteht darin, den Prozess, der das DSC-Modul hostet, explizit zu beenden.

Ähnliches gilt, wenn Sie `Start-DscConfiguration` nach dem Hinzufügen und Ändern einer benutzerdefinierten Ressource ausführen. Die Änderung kann dann möglicherweise nicht ausgeführt werden, bis der Computer neu gestartet wird. Grund hierfür ist, dass DSC im WMI-Anbieterhostprozess (WmiPrvSE) ausgeführt wird, und in der Regel mehrere Instanzen von WmiPrvSE gleichzeitig ausgeführt werden. Beim Neustart wird der Hostprozess neu gestartet und der Cache geleert.

Um die Konfiguration erfolgreich zu recyceln und den Cache zu löschen, ohne einen Neustart auszuführen, müssen Sie den Hostprozess beenden und neu starten. Dazu können Sie den Prozess auf Instanzebene identifizieren, beenden und neu starten. Sie können auch `DebugMode` verwenden, wie nachfolgend gezeigt, um die PowerShell DSC-Ressource erneut zu laden.

Um zu identifizieren, welcher Prozess das DSC-Modul hostet, und diesen auf Instanzebene zu beenden, können Sie die Prozess-ID des WmiPrvSE-Prozesses auflisten, der das DSC-Modul hostet. Beenden Sie den WmiPrvSE-Prozess dann mithilfe der unten aufgeführten Befehle, um den Anbieter zu aktualisieren, und führen Sie anschließend **Start-DscConfiguration** erneut aus.

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

## Verwenden von DebugMode

Sie können den lokalen Konfigurations-Manager (LCM) von DSC für die Verwendung von `DebugMode` konfigurieren, damit der Cache bei jedem Neustart des Hostprozesses neu gestartet wird. Indem Sie den Debugmodus verwenden (auf **TRUE** festlegen), lädt das Modul die PowerShell DSC-Ressource immer neu. Sobald Sie mit dem Schreiben Ihrer Ressource fertig sind, können Sie den Debugmodus wieder deaktivieren (auf **FALSE** festlegen), um zum alten Verhalten zurückzukehren und die Module wieder zwischenzuspeichern.

In der folgenden Demonstration wird verdeutlicht, wie `DebugMode` den Cache automatisch aktualisieren kann. Betrachten wir zunächst die Standardkonfiguration:

```
PS C:\> Get-DscLocalConfigurationManager
 
 
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

Sie sehen, dass `DebugMode` auf **FALSE** festgelegt ist.

Zum Einrichten der `DebugMode`-Demo verwenden Sie die folgenden PowerShell-Ressource:

```powershell
function Get-TargetResource
{
    param
    (
        [Parameter(Mandatory)]
        $onlyProperty
    )
    return @{onlyProperty = Get-Content -Path "$env:SystemDrive\OutputFromTestProviderDebugMode.txt"}
}
function Set-TargetResource
{
    param
    (
        [Parameter(Mandatory)]
        $onlyProperty
    )
    "1" | Out-File -PSPath "$env:SystemDrive\OutputFromTestProviderDebugMode.txt"
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

Nun erstellen Sie eine Konfiguration mit der oben stehenden Ressource namens `TestProviderDebugMode`:

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

Sie erkennen, dass der Inhalt der Datei „**$env:SystemDrive\OutputFromTestProviderDebugMode.txt**“ **1** ist.

Aktualisieren Sie nun den Anbietercode mithilfe des folgenden Skripts:

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
    return @{onlyProperty = Get-Content -Path "$env:SystemDrive\OutputFromTestProviderDebugMode.txt"}
}
function Set-TargetResource
{
    param
    (
        [Parameter(Mandatory)]
        `$onlyProperty
    )
    "$newResourceOutput" | Out-File -PSPath C:\OutputFromTestProviderDebugMode.txt
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

Dieses Skript generiert eine Zufallszahl und aktualisiert den Anbietercode entsprechend. Ist `DebugMode` auf „false“ festgelegt, wird der Inhalt der Datei „**$env:SystemDrive\OutputFromTestProviderDebugMode.txt**“ nie geändert.

Legen Sie `DebugMode` in Ihrem Konfigurationsskript jetzt auf **TRUE** fest:

```powershell
LocalConfigurationManager
{
    DebugMode = $true
} 
```

Wenn Sie das oben stehende Skript erneut ausführen, sehen Sie, dass der Inhalt der Datei jedes Mal anders ist. (Zum Überprüfen können Sie `Get-DscConfiguration` ausführen). Im Folgenden wird das Ergebnis von zwei weiteren Ausführungen angezeigt (Ihre Ergebnisse können sich nach Ausführen des Skripts von diesem Ergebnis unterscheiden):

```powershell
PS C:\> Get-DscConfiguration -CimSession (New-CimSession localhost)
 
onlyProperty                            PSComputerName                         
------------                            --------------                         
20                                      localhost                              
 
PS C:\> Get-DscConfiguration -CimSession (New-CimSession localhost)
 
onlyProperty                            PSComputerName                         
------------                            --------------                         
14                                      localhost
```

## Weitere Informationen

### Verweis
* [DSC-Protokollressource](logResource.md)

### Konzepte
* [Erstellen von benutzerdefinierten Windows PowerShell DSC-Ressourcen](authoringResource.md)

### Weitere Ressourcen
* [Windows PowerShell DSC-Cmdlets](https://technet.microsoft.com/en-us/library/dn521624(v=wps.630).aspx)




<!--HONumber=Oct16_HO1-->


