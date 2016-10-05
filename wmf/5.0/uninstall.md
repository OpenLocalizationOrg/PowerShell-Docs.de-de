# Deinstallationsanweisungen

## Verwenden der Eingabeaufforderung
1.  Öffnen Sie die **Eingabeaufforderung**.
2.  Führen Sie das [eigenständige Windows Update-Startprogramm](https://support.microsoft.com/en-us/kb/934307) wie unten dargestellt aus:

Unter Windows Server 2012 R2 und Windows 8.1:
```powershell
wusa /uninstall /kb:3134758
```
Unter Windows Server 2012:
```powershell
wusa /uninstall /kb:3134759
```
Unter Windows Server 2008 R2 SP1 und Windows 7 SP1:
```powershell
wusa /uninstall /kb:3134760
```

## Über die Systemsteuerung
1.  Öffnen Sie die **Systemsteuerung**.
2.  Öffnen Sie **Programme** und dann **Programm deinstallieren**.
3.  Klicken Sie auf **Installierte Updates anzeigen**.
4.  Wählen Sie in der Liste der installierten Updates **Windows Management Framework 5.0** aus. Dies entspricht *KB3134758*, *KB3134759* oder *KB3134760*. Klicken Sie auf **Deinstallieren**.


<!--HONumber=Oct16_HO1-->


