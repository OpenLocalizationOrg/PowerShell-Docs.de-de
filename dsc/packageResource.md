#DSC Paketressource

> Gilt für: WindowsPowerShell 4.0, WindowsPowerShell 5.0

Die **Paket** Ressource in Windows PowerShell gewünscht State Configuration (DSC) bietet einen Mechanismus zum Installieren oder Deinstallieren von Paketen, wie z. B. Windows Installer und setup.exe-Pakete auf dem Zielknoten.

##Syntax

```
Package [string] #ResourceName
{
    Name = [string]
    Path = [string]
    ProductId = [string]
    [ Arguments = [string] ]
    [ Credential = [PSCredential] ]
    [ Ensure = [string] { Absent | Present }  ]
    [ LogPath = [string] ]
    [ DependsOn = [string[]] ]
    [ ReturnCode = [UInt32[]] ]
}
```

##Eigenschaften

| Eigenschaft| Beschreibung|
|---|---|
| Name| Gibt den Namen des Pakets zu einen bestimmten Zustand sichergestellt werden soll.|
| Pfad| Gibt den Pfad, in dem das Paket gespeichert ist.|
| ProductId| Gibt die Produkt-ID, die das Paket eindeutig identifiziert.|
| Argumente| Führt eine Zeichenfolge von Argumenten, die das Paket genau wie angegeben übergeben wird.|
| Credential| Ermöglicht den Zugriff auf das Paket auf einem remote-Quelle.Diese Eigenschaft wird nicht verwendet, um das Paket zu installieren.Das Paket wird immer auf dem lokalen System installiert.|
| Stellen Sie sicher| Gibt an, ob das Paket installiert ist.Legen Sie diese Eigenschaft auf "Abwesend", um sicherzustellen, dass das Paket nicht installiert ist (oder Deinstallieren des Pakets, wenn es installiert ist).Legen Sie ihn (Standardwert) "vorhanden", um sicherzustellen, dass das Paket installiert ist.|
| LogPath| Gibt den vollständigen Pfad, den Anbieter eine Protokolldatei zum Installieren oder deinstallieren das Paket gespeichert werden soll.|
| DependsOn| Gibt an, dass die Konfiguration von einer anderen Ressource ausgeführt werden muss, bevor diese Ressource konfiguriert wird.Ist z. B. wenn die ID der Ressourcenkonfiguration Block Skripts, die Sie ausführen möchten zuerst **ResourceName** und der Typ ist **ResourceType**, die Syntax für die Verwendung dieser Eigenschaft ist "DependsOn ="[ResourceType] ResourceName"".|
| Rückgabecode| Gibt den erwarteten Rückgabecode an.Wenn Sie den tatsächlichen Code zurück, entspricht nicht dem erwarteten Wert, finden Sie hier die Konfiguration einen Fehler zurück.|

##Beispiel

In diesem Beispiel wird das MSI-Installationsprogramm, das befindet sich unter dem angegebenen Pfad und die angegebenen Produkt-ID

```powershell
Package PackageExample
{
    Ensure = "Present"  # You can also set Ensure to "Absent"
    Path  = "$Env:SystemDrive\TestFolder\TestProject.msi"
    Name = "TestPackage"
    ProductId = "ACDDCDAF-80C6-41E6-A1B9-8ABD8A05027E"
} 
```




