#Schreiben eine benutzerdefinierte DSC-Ressource mit MOF

> Gilt für: WindowsPowerShell 4.0, WindowsPowerShell 5.0

In diesem Thema werden wir das Schema für eine benutzerdefinierte Windows PowerShell gewünscht State Configuration (DSC)-Ressource in eine MOF-Datei definieren und implementieren die Ressource in einer Windows PowerShell-Skriptdatei. Diese benutzerdefinierte Ressource ist zum Erstellen und Verwalten einer Website.

##Erstellen des MOF-Schemas

Das Schema definiert die Eigenschaften der Ressource, die durch ein Konfigurationsskript DSC konfiguriert werden können.

###Ordnerstruktur für eine MOF-Ressource

Um eine benutzerdefinierte DSC-Ressource mit einer MOF-Schema zu implementieren, erstellen Sie die folgende Ordnerstruktur. Das MOF-Schema wird in der Demo-Datei definiert_IISWebsite.schema.mof und das Ressourcenskript wird definiert, in der Demo_IISWebsite.ps1. Optional können Sie eine Modul-Manifestdatei (psd1)-Datei erstellen.

```
$env: psmodulepath (folder)
    |- MyDscResources (folder)
        |- DSCResources (folder)
            |- Demo_IISWebsite (folder)
                |- Demo_IISWebsite.psd1 (file, optional)
                |- Demo_IISWebsite.psm1 (file, required)
                |- Demo_IISWebsite.schema.mof (file, required)
```

Beachten Sie, dass es notwendig ist, erstellen Sie einen Ordner mit dem Namen DSCResources im Ordner der obersten Ebene und der Ordner für jede Ressource den gleichen Namen wie die Ressource aufweisen muss.

###Der Inhalt der MOF-Datei

Es folgt ein Beispiel-MOF-Datei, die für eine benutzerdefinierte Website-Ressource verwendet werden kann. Um dieses Beispiel auszuführen, dieses Schema in einer Datei speichern und die Datei *Demo_IISWebsite.schema.mof*.

```
[ClassVersion("1.0.0"), FriendlyName("Website")] 
class Demo_IISWebsite : OMI_BaseResource
{
  [Key] string Name;
  [Required] string PhysicalPath;
  [write,ValueMap{"Present", "Absent"},Values{"Present", "Absent"}] string Ensure;
  [write,ValueMap{"Started","Stopped"},Values{"Started", "Stopped"}] string State;
  [write] string Protocol[];
  [write] string BindingInfo[];
  [write] string ApplicationPool;
  [read] string ID;
};
```

Beachten Sie die folgenden Informationen zu den vorherigen Code:

* `FriendlyName` definiert den Namen, die auf diese benutzerdefinierte Ressource in Konfigurationsskripts DSC verweisen können. In diesem Beispiel `Website` ist gleichbedeutend mit dem Anzeigenamen `Archiv` für die integrierten Archiv-Ressource.
* Die Klasse für Ihre benutzerdefinierte Ressource von abgeleitet werden, müssen Sie definieren `OMI_BaseResource`.
* Der Typqualifizierer `[Key]`, auf eine Eigenschaft gibt an, dass diese Eigenschaft die Resource-Instanz eindeutig identifiziert wird. Ein `[Key]` Eigenschaft ist ebenfalls erforderlich.
* Die `[Required]` -Kennzeichner gibt an, dass die Eigenschaft erforderlich ist (ein Wert muss in jedem Configuration-Skript, das diese Ressource verwendet angegeben werden).
* Die `[schreiben]` -Kennzeichner gibt an, dass diese Eigenschaft optional ist, wenn Sie benutzerdefinierte Ressource in ein Konfigurationsskript verwenden. Die `[read]` -Kennzeichner gibt an, dass eine Eigenschaft kann nicht von einer Konfiguration festgelegt werden, und nur für Berichtszwecke.
* `Werte` schränkt die Werte, die die Eigenschaft, die die Liste der definierten Werte zugewiesen werden, können `ValueMap`. Weitere Informationen finden Sie unter [ValueMap und Wert Qualifizierer](https://msdn.microsoft.com/library/windows/desktop/aa393965.aspx).
* Einschließlich einer Eigenschaft namens `sicherstellen, dass` in die Ressource als Möglichkeit zum Verwalten eines konsistenten Stils mit integrierten DSC-Ressourcen empfohlen.
* Benennen Sie die Schemadatei für Ihre benutzerdefinierte Ressource wie folgt: `classname.schema.mof`, wobei `Classname` ist der Bezeichner, der folgt der `Klasse` Schlüsselwort in der Schemadefinition.

###Schreiben die Ressourcenskriptdatei

Das Ressourcenskript implementiert die Logik der Ressource. In diesem Modul müssen Sie drei aufgerufenen Funktionen einschließen **Get-TargetResource**, **Set TargetResource**, und **Test TargetResource**. Alle drei Funktionen müssen einen Parametersatz erstellen, der identisch mit dem Satz von Eigenschaften, die im MOF-Schema, das Sie für die Ressource erstellt definiert ist. In diesem Dokument wird dieser Satz von Eigenschaften als die "Ressourceneigenschaften." bezeichnet. Speicher, die diese drei Funktionen in einer Datei namens <ResourceName>psm1. Im folgenden Beispiel werden die Funktionen in einer Datei namens Demo_IISWebsite.psm1 gespeichert.

> **Hinweis**: Wenn Sie das gleiche Skript mehr als einmal auf die Ressource ausführen, Sie keine Fehlermeldungen angezeigt sollten und die Ressource in denselben Zustand wie nach Ausführung des Skripts bleiben sollen. Um dies zu erreichen, stellen Sie sicher, Ihre **Get-TargetResource** und **Test TargetResource** Funktionen unverändert die Ressource, und diese Aufrufen der **Set TargetResource** Funktion mehr als einmal in einer Sequenz mit dem gleichen Parameter Werte entspricht immer einmal aufrufen.

In der **Get-TargetResource** Implementierung-Funktion, die wichtigsten Eigenschaftswerte, die bereitgestellt werden als Parameter Überprüfen des Status der Instanz angegebene Ressource. Diese Funktion muss eine Hashtabelle zurück, die die Ressourceneigenschaften als Schlüssel und die tatsächlichen Werte dieser Eigenschaften als die entsprechenden Werte aufgeführt. Der folgende Code enthält ein Beispiel.

```powershell
# DSC uses the Get-TargetResource function to fetch the status of the resource instance specified in the parameters for the target machine
function Get-TargetResource 
{
    param 
    (       
        [ValidateSet("Present", "Absent")]
        [string]$Ensure = "Present",

        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [string]$Name,

        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [string]$PhysicalPath,

        [ValidateSet("Started", "Stopped")]
        [string]$State = "Started",

        [string]$ApplicationPool,

        [string[]]$BindingInfo,

        [string[]]$Protocol
    )

        $getTargetResourceResult = $null;

        <# Insert logic that uses the mandatory parameter values to get the website and assign it to a variable called $Website #>
        <# Set $ensureResult to "Present" if the requested website exists and to "Absent" otherwise #>

        # Add all Website properties to the hash table
        # This simple example assumes that $Website is not null
        $getTargetResourceResult = @{
                                      Name = $Website.Name; 
                                        Ensure = $ensureResult;
                                        PhysicalPath = $Website.physicalPath;
                                        State = $Website.state;
                                        ID = $Website.id;
                                        ApplicationPool = $Website.applicationPool;
                                        Protocol = $Website.bindings.Collection.protocol;
                                        Binding = $Website.bindings.Collection.bindingInformation;
                                    }

        $getTargetResourceResult;
}
```

Abhängig von den Werten, die für die Ressourceneigenschaften in das Konfigurationsskript angegeben sind die **Set TargetResource** müssen, führen Sie eine der folgenden:

* Erstellen einer neuen website
* Aktualisieren einer vorhandenen website
* Löschen einer vorhandenen website

Das folgende Beispiel veranschaulicht dies.

```powershell
# The Set-TargetResource function is used to create, delete or configure a website on the target machine. 
function Set-TargetResource 
{
    [CmdletBinding(SupportsShouldProcess=$true)]
    param 
    (       
        [ValidateSet("Present", "Absent")]
        [string]$Ensure = "Present",

        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [string]$Name,

        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [string]$PhysicalPath,

        [ValidateSet("Started", "Stopped")]
        [string]$State = "Started",

        [string]$ApplicationPool,

        [string[]]$BindingInfo,

        [string[]]$Protocol
    )

    <# If Ensure is set to "Present" and the website specified in the mandatory input parameters does not exist, then create it using the specified parameter values #>
    <# Else, if Ensure is set to "Present" and the website does exist, then update its properties to match the values provided in the non-mandatory parameter values #>
    <# Else, if Ensure is set to "Absent" and the website does not exist, then do nothing #>
    <# Else, if Ensure is set to "Absent" and the website does exist, then delete the website #>
}
```

Abschließend die **Test TargetResource** Funktion muss denselben als Parameter akzeptieren **Get-TargetResource** und **Set TargetResource**. In der Implementierung von **Test TargetResource**, überprüfen Sie den Status der Ressourceninstanz, die in die wichtigsten Parameter angegeben ist. Wenn der tatsächliche Status der Ressource nicht im Satz der Parameter angegebenen Werte übereinstimmt, zurück **$false**. Andernfalls zurück **$true**.

Der folgende code implementiert die **Test TargetResource** Funktion.

```powershell
function Test-TargetResource
{
[CmdletBinding()]
[OutputType([System.Boolean])]
param
(
[ValidateSet("Present","Absent")]
[System.String]
$Ensure,

[parameter(Mandatory = $true)]
[System.String]
$Name,

[parameter(Mandatory = $true)]
[System.String]
$PhysicalPath,

[ValidateSet("Started","Stopped")]
[System.String]
$State,

[System.String[]]
$Protocol,

[System.String[]]
$BindingData,

[System.String]
$ApplicationPool
)

#Write-Verbose "Use this cmdlet to deliver information about command processing."

#Write-Debug "Use this cmdlet to write debug information while troubleshooting."


#Include logic to 
$result = [System.Boolean]
#Add logic to test whether the website is present and its status mathes the supplied parameter values. If it does, return true. If it does not, return false.
$result 
}
```

**Hinweis**: einfacher debuggen, verwenden Sie die **Write-Verbose** Cmdlet in der Implementierung der vorherigen drei Funktionen. Dieses Cmdlet schreibt Text in den Stream ausführliche Meldung. Standardmäßig der Datenstrom ausführliche Meldung wird nicht angezeigt, aber es können Sie anzeigen, indem Sie den Wert der **$VerbosePreference** Variable oder mithilfe der **ausführlich** Parameter in den Cmdlets DSC = new.

###Erstellen das modulmanifest

Verwenden Sie schließlich die **New-ModuleManifest** -Cmdlet zum Definieren einer <ResourceName>psd1-Datei für Ihre benutzerdefinierte Ressource-Modul. Wenn Sie dieses Cmdlet aufrufen, verweisen Sie auf die Skript-Modul (psm1)-Datei, die im vorherigen Abschnitt beschrieben. Umfassen **Get-TargetResource**, **Set TargetResource**, und **Test TargetResource** in der Liste der Funktionen für den export. Es folgt ein Beispiel-Manifestdatei.

```powershell
# Module manifest for module 'Demo.IIS.Website'
#
# Generated on: 1/10/2013
#

@{

# Script module or binary module file associated with this manifest.
# RootModule = ''

# Version number of this module.
ModuleVersion = '1.0'

# ID used to uniquely identify this module
GUID = '6AB5ED33-E923-41d8-A3A4-5ADDA2B301DE'

# Author of this module
Author = 'Contoso'

# Company or vendor of this module
CompanyName = 'Contoso'

# Copyright statement for this module
Copyright = 'Contoso. All rights reserved.'

# Description of the functionality provided by this module
Description = 'This Module is used to support the creation and configuration of IIS Websites through Get, Set and Test API on the DSC managed nodes.'

# Minimum version of the Windows PowerShell engine required by this module
PowerShellVersion = '4.0'

# Minimum version of the common language runtime (CLR) required by this module
CLRVersion = '4.0'

# Modules that must be imported into the global environment prior to importing this module
RequiredModules = @("WebAdministration")

# Modules to import as nested modules of the module specified in RootModule/ModuleToProcess
NestedModules = @("Demo_IISWebsite.psm1")

# Functions to export from this module
FunctionsToExport = @("Get-TargetResource", "Set-TargetResource", "Test-TargetResource")

# Cmdlets to export from this module
#CmdletsToExport = '*'

# HelpInfo URI of this module
# HelpInfoURI = ''
}
```



