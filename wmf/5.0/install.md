# Installationsanweisungen

Laden Sie das richtige Paket für Ihr Betriebssystem und Ihre Architektur herunter:

| Betriebssystem       | Architektur | Paketname              | 
|------------------------|--------------|---------------------------| 
| Windows Server 2012 R2 | x64      | [Win8.1AndW2K12R2-KB3134758-x64.msu](http://go.microsoft.com/fwlink/?LinkId=717507) | 
| Windows Server 2012    | x64      | [W2K12-KB3134759-x64.msu](http://go.microsoft.com/fwlink/?LinkId=717506) | 
| Windows Server 2008 R2 | x64      | [Win7AndW2K8R2-KB3134760-x64.msu](http://go.microsoft.com/fwlink/?LinkId=717504) |
| Windows 8.1            | x64          | [Win8.1AndW2K12R2-KB3134758-x64.msu](http://go.microsoft.com/fwlink/?LinkId=717507) |
| Windows 8.1            | x86          | [Win8.1-KB3134758-x86.msu](http://go.microsoft.com/fwlink/?LinkID=717963) |
| Windows 7 SP1          | x64          | [Win7AndW2K8R2-KB3134760-x64.msu](http://go.microsoft.com/fwlink/?LinkId=717504) |
| Windows 7 SP1          | x86          | [Win7-KB3134760-x86.msu](http://go.microsoft.com/fwlink/?LinkID=717962) |


**So installieren WMF 5.0 von Windows Explorer (oder Datei-Explorer in Windows Server 2012 R2 und Windows 8.1):**

1. Wechseln Sie zum Ordner, in den Sie die MSU-Datei heruntergeladen haben.

2. Doppelklicken Sie auf die MSU-Datei, um sie auszuführen.

**So installieren Sie WMF 5.0 über Befehlszeile** 

1. Öffnen Sie nach dem Herunterladen des richtigen Pakets für die Architektur Ihres Computers ein Eingabeaufforderungsfenster mit erhöhten Benutzerrechten (Als Administrator ausführen). Bei den Server Core-Installationsoptionen von Windows Server 2012 R2, Windows Server 2012 oder Windows Server 2008 R2 SP1 wird das Eingabeaufforderungsfenster standardmäßig mit erhöhten Benutzerrechten geöffnet.

2. Wechseln Sie zum Ordner, in den Sie das Installationspaket für WMF 5.0 heruntergeladen oder kopiert haben.

3. Führen Sie einen der folgenden Befehle aus:
    - Führen Sie auf Computern mit Windows Server 2012 R2 oder Windows 8.1 x64 **Win8.1AndW2K12R2-KB3134758-x64.msu /quiet** aus.
    - Führen Sie auf Computern mit Windows Server 2012 **W2K12-KB3134759-x64.msu /quiet** aus.
    - Führen Sie auf Computern mit Windows Server 2008 R2 SP1 oder Windows 7 SP1 x64 **Win7AndW2K8R2-KB3134760-x64.msu /quiet** aus.
    - Führen Sie auf Computern mit Windows 8.1 x86 **Win8.1-KB3134758-x86.msu /quiet** aus.
    - Führen Sie auf Computern mit Windows 7 SP1 x86 **Win7-KB3134760-x86.msu /quiet** aus.

**Zusätzliche Installationshinweise für Windows Server 2008 SP1 und Windows 7 SP1:**

Überprüfen Sie, ob die folgenden Voraussetzungen erfüllt sind:
- Das neueste Service Pack ist installiert.
- [WMF 4.0](http://www.microsoft.com/en-us/download/details.aspx?id=40855) ist installiert.

*WinRM-Abhängigkeit:* Windows PowerShell DSC (Desired State Configuration) hängt von WinRM ab. Unter Windows Server 2008 R2 und Windows 7 ist WinRM nicht standardmäßig aktiviert. Führen Sie zum Aktivieren von WinRM in einer Windows PowerShell-Sitzung mit erhöhten Benutzerrechten **Set-WSManQuickConfig** aus.




<!--HONumber=Oct16_HO1-->


