# Verwalten von Netzwerkswitches mit PowerShell

Das Cmdlet **Get-NetworkSwitchEthernetPort** gibt nun die folgenden zusätzlichen Informationen für Instanzen zurück:
-   „IPAddress“ – die dem Port zugeordnete IP-Adresse
-   PortMode – der Portmodus: „access“, „route“ oder „trunk“
-   AccessVLAN – die diesem Port im Modus „access“ zugeordnete ID des VLAN
-   TrunkedVLANList – die diesem Port im Modus „trunk“ zugeordnete Listen von IDs von VLANs

## Grundlegende Verwaltung von Netzwerkswitches mit Windows PowerShell
Die „NetworkSwitch“-Cmdlets, die in der ersten WMF 5.0 Preview-Version eingeführt wurden, ermöglichen Ihnen Netzwerkswitches, die mit dem Windows Server 2012 R2-Logo zertifiziert sind, mit einer Switch-, VLAN- und grundlegenden Layer 2-Netzwerkswitch-Konfiguration zu versehen. Microsoft unterstützt weiterhin das [Datacenter Abstraction Layer](http://technet.microsoft.com/en-us/cloud/dal.aspx)-Konzept (DAL), um Kunden und Partnern weiter einen Nutzen zu bieten. Mithilfe dieser Cmdlets können Sie Folgendes ausführen:

-   Globale Switchkonfiguration, wie z. B.:
    -   Hostnamen festlegen
    -   Switchbanner festlegen
    -   Konfiguration dauerhaft speichern
    -   Features aktivieren oder deaktivieren

-   VLAN-Konfiguration:
    -   VLAN erstellen oder entfernen
    -   VLAN aktivieren oder deaktivieren
    -   VLAN auflisten
    -   Anzeigenamen für ein VLAN festlegen

-   Layer 2-Portkonfiguration:
    -   Ports auflisten
    -   Ports aktivieren oder deaktivieren
    -   Portmodi und -eigenschaften festlegen
    -   VLAN im Modus „Access“ oder „Trunk“ für den Port hinzufügen oder zuordnen

Machen Sie sich am besten mit allen „NetworkSwitch“-Cmdlets vertraut!

```powershell
PS> Get-Command *-NetworkSwitch*

| CommandType | Name                                      | Source        |
|-------------|-------------------------------------------|---------------|
|             |                                           |               |
| Function    | Disable-NetworkSwitchEthernetPort         | NetworkSwitch |
| Function    | Disable-NetworkSwitchFeature              | NetworkSwitch |
| Function    | Disable-NetworkSwitchVlan                 | NetworkSwitch |
| Function    | Enable-NetworkSwitchEthernetPort          | NetworkSwitch |
| Function    | Enable-NetworkSwitchFeature               | NetworkSwitch |
| Function    | Enable-NetworkSwitchVlan                  | NetworkSwitch |
| Function    | Get-NetworkSwitchEthernetPort             | NetworkSwitch |
| Function    | Get-NetworkSwitchFeature                  | NetworkSwitch |
| Function    | Get-NetworkSwitchGlobalData               | NetworkSwitch |
| Function    | Get-NetworkSwitchVlan                     | NetworkSwitch |
| Function    | New-NetworkSwitchVlan                     | NetworkSwitch |
| Function    | Remove-NetworkSwitchEthernetPortIPAddress | NetworkSwitch |
| Function    | Remove-NetworkSwitchVlan                  | NetworkSwitch |
| Function    | Restore-NetworkSwitchConfiguration        | NetworkSwitch |
| Function    | Save-NetworkSwitchConfiguration           | NetworkSwitch |
| Function    | Set-NetworkSwitchEthernetPortIPAddress    | NetworkSwitch |
| Function    | Set-NetworkSwitchPortMode                 | NetworkSwitch |
| Function    | Set-NetworkSwitchPortProperty             | NetworkSwitch |
| Function    | Set-NetworkSwitchVlanProperty             | NetworkSwitch |
```

Weitere Informationen finden im Blogbeitrag von Jeffrey Snover mit der Ankündigung der WMF 5.0 Preview-Version: <http://blogs.technet.com/b/windowsserver/archive/2014/04/03/windows-management-framework-v5-preview.aspx>


<!--HONumber=Oct16_HO1-->


