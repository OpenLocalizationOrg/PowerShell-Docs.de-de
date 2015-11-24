#Zusammengesetzte Ressourcen: mit einer DSC-Konfiguration als Ressource

> Gilt für: WindowsPowerShell 4.0, WindowsPowerShell 5.0

In realen Situationen können Konfigurationen werden lange und komplexe, viele verschiedene Ressourcen aufrufen und viele andere Eigenschaften festlegen. Um helfen, diese Komplexität zu beheben, können Sie eine Konfiguration für Windows PowerShell gewünscht State Configuration (DSC) für andere Konfigurationen als Ressource. Wir nennen dies eine zusammengesetzte Ressource. Eine zusammengesetzte Ressource ist eine DSC-Konfiguration, die Parameter akzeptiert. Die Parameter der Konfiguration Verhalten sich wie die Eigenschaften der Ressource. Die Konfiguration wird gespeichert, als eine Datei mit einem **. Schema.psm1**-Erweiterung und nimmt der Platz von MOF-Schema und die Ressource ein Skript in einer normalen DSC-Ressource (Weitere Informationen zu DSC-Ressourcen finden Sie unter [Windows PowerShell gewünschten Status Konfigurationsressourcen](resources.md).

##Zusammengesetzte Ressource erstellen

In unserem Beispiel erstellen wir eine Konfiguration, die eine Reihe von vorhandenen Ressourcen zum Konfigurieren von virtuellen Maschinen aufruft. Anstelle der Werte in Blöcken Konfiguration festgelegt werden, nimmt die Konfiguration eine Reihe von Parametern, die dann in die Konfiguration Blöcke verwendet werden.

```powershell
Configuration xVirtualMachine
{
param
(
# Name of VMs
[Parameter(Mandatory)]
[ValidateNotNullOrEmpty()]
[String[]]$VMName,

# Name of Switch to create
[Parameter(Mandatory)]
[ValidateNotNullOrEmpty()]
[String]$SwitchName,

# Type of Switch to create
[Parameter(Mandatory)]
[ValidateNotNullOrEmpty()]
[String]$SwitchType,

# Source Path for VHD
[Parameter(Mandatory)]
[ValidateNotNullOrEmpty()]
[String]$VhdParentPath,

# Destination path for diff VHD
[Parameter(Mandatory)]
[ValidateNotNullOrEmpty()]
[String]$VHDPath,

# Startup Memory for VM
[Parameter(Mandatory)]
[ValidateNotNullOrEmpty()]
[String]$VMStartupMemory,

# State of the VM
[Parameter(Mandatory)]
[ValidateNotNullOrEmpty()]
[String]$VMState
)

# Import the module that defines custom resources
Import-DscResource -Module xComputerManagement,xHyper-V

# Install the HyperV role 
WindowsFeature HyperV
{
    Ensure = "Present"
    Name = "Hyper-V" 
}

# Create the virtual switch 
xVMSwitch $switchName
{
    Ensure = "Present"
    Name = $switchName
    Type = $SwitchType
    DependsOn = "[WindowsFeature]HyperV"
}

# Check for Parent VHD file
File ParentVHDFile
{
    Ensure = "Present"
    DestinationPath = $VhdParentPath
    Type = "File"
    DependsOn = "[WindowsFeature]HyperV"
}

# Check the destination VHD folder
File VHDFolder
{
    Ensure = "Present"
    DestinationPath = $VHDPath
    Type = "Directory"
    DependsOn = "[File]ParentVHDFile"
}

 # Creae VM specific diff VHD
foreach($Name in $VMName)
{
    xVHD "VhD$Name"
    {
        Ensure = "Present"
        Name = $Name
        Path = $VhDPath
        ParentPath = $VhdParentPath
        DependsOn = @("[WindowsFeature]HyperV",
                      "[File]VHDFolder")
    }
}

# Create VM using the above VHD
foreach($Name in $VMName)
{
    xVMHyperV "VMachine$Name"
    {
        Ensure = "Present"
        Name = $Name
        VhDPath = (Join-Path -Path $VhDPath -ChildPath $Name)
        SwitchName = $SwitchName
        StartupMemory = $VMStartupMemory
        State = $VMState
        MACAddress = $MACAddress 
        WaitForIP = $true
        DependsOn = @("[WindowsFeature]HyperV",
                      "[xVHD]Vhd$Name")
    }
} 
}
```

###Speichern die Konfiguration als zusammengesetzte Ressource

Um die parametrisierte Konfiguration als DSC Ressource zu verwenden, speichern Sie sie in einer Verzeichnisstruktur wie die anderen MOF-basierte Ressource und nennen sie mit einem **. schema.psm1** Erweiterung. In diesem Beispiel werden wir nennen Sie die Datei **xVirtualMachine.schema.psm1**. Müssen Sie außerdem erstellen ein Manifest mit der Bezeichnung **xVirtualMachine.psd1** die die folgende Zeile enthält. Beachten Sie, dass dies zusätzlich zu **MyDscResources.psd1**, das modulmanifest für alle Ressourcen unter den **MyDscResources** Ordner.

```powershell
RootModule = 'xVirtualMachine.schema.psm1'
```

Wenn Sie fertig sind, sollte die Ordnerstruktur wie folgt aussehen.

```
$env: psmodulepath
    |- MyDscResources 
           MyDscResources.psd1
        |- DSC Resources 
            |- xVirtualMachine
                |- xVirtualMachine.psd1 
                |- xVirtualMachine.schema.psm1
```

Die Ressource wird mit dem Cmdlet Get-DscResource ermittelt, und seine Eigenschaften ermittelt, indem entweder das Cmdlet oder **STRG + LEERTASTE** automatische Vervollständigung in Windows PowerShell ISE.

##Verwenden die zusammengesetzte Ressource

Als Nächstes erstellen wir eine Konfiguration, die die zusammengesetzte Ressource aufruft. Diese Konfiguration Ruft die xVirtualMachine-Ressource, um einen virtuellen Computer erstellen und dann die **xComputer** Ressource umzubenennen.

```powershell
configuration RenameVM
{
Import-DscResource -Module TestCompositeResource

Node localhost
{
    xVirtualMachine VM
    {
        VMName = "Test"
        SwitchName = "Internal"
        SwitchType = "Internal"
        VhdParentPath = "C:\Demo\Vhd\RTM.vhd"
        VHDPath = "C:\Demo\Vhd"
        VMStartupMemory = 1024MB
        VMState = "Running"
    }
    }
   Node "192.168.10.1"
   {   
    xComputer Name
    {
        Name = "SQL01"
        DomainName = "fourthcoffee.com" 
    }                                                                                                                                                                                                                                                               
}
} 
```

##Weitere Informationen

###Konzepte

* [Schreiben eine benutzerdefinierte DSC-Ressource mit MOF](authoringResourceMOF.md)
* [Erste Schritte mit Windows PowerShell gewünschten Status-Konfiguration](overview.md)



