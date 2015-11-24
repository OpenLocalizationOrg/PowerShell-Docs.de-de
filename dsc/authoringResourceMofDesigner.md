#Mithilfe des Tools Ressourcen-Designer

> Gilt für: WindowsPowerShell 4.0, WindowsPowerShell 5.0

Mit dem Ressourcen-Designer ist ein Satz von Cmdlets, die verfügbar gemacht werden, indem die **xDscResourceDesigner** -Modul, das Erstellen von Windows PowerShell gewünscht State Configuration (DSC) Ressourcen zu erleichtern. Die Cmdlets in dieser Ressource können das MOF-Schema, das Skriptmodul und die Verzeichnisstruktur für die neue Ressource zu erstellen. Weitere Informationen zu DSC-Ressourcen finden Sie unter [benutzerdefinierte Windows PowerShell gewünschten Status Konfiguration Buildressourcen](authoringResource.md).
In diesem Thema erstellen wir eine DSC-Ressource, die Active Directory-Benutzer zu verwalten.
Verwenden der [Install-Module](https://technet.microsoft.com/en-us/library/dn807162.aspx) -Cmdlet zum Installieren der **xDscResourceDesigner** Modul.

> **Hinweis**: **Install-Module** befindet sich auf der **PowerShellGet** Module, die in PowerShell 5.0 enthalten ist. Download der **PowerShellGet** -Modul für PowerShell 3.0 und 4.0 auf [PackageManagement PowerShell-Module Vorschau](https://www.microsoft.com/en-us/download/details.aspx?id=49186).

##Erstellen von Ressourceneigenschaften

Als Erstes müssen wir ist Eigenschaften festlegen, die die Ressource verfügbar macht. In diesem Beispiel definieren wir einen Active Directory-Benutzer mit den folgenden Eigenschaften.

Parametername Beschreibung
* **Benutzername**: Schlüsseleigenschaft, der ein Benutzer eindeutig identifiziert.
* **Sicherstellen, dass**: Gibt an, ob das Benutzerkonto vorhanden sein soll oder nicht vorhanden ist. Dieser Parameter wird nur zwei mögliche Werte aufweisen.
* **DomainCredential**: das Kennwort für den Benutzer.
* **Kennwort**: das gewünschte Kennwort für den Benutzer zu einer Konfiguration, um das Kennwort bei Bedarf ändern.

Um die Eigenschaften zu erstellen, verwenden wir die **neu xDscResourceProperty** Cmdlet. Die folgenden PowerShell-Befehle erstellen, die oben beschriebenen Eigenschaften.

```powershell
PS C:\> $UserName = New-xDscResourceProperty –Name UserName -Type String -Attribute Key
PS C:\> $Ensure = New-xDscResourceProperty –Name Ensure -Type String -Attribute Write –ValidateSet “Present”, “Absent”
PS C:\> $DomainCredential = New-xDscResourceProperty –Name DomainCredential-Type PSCredential -Attribute Write
PS C:\> $Password = New-xDscResourceProperty –Name Password -Type PSCredential -Attribute Write
```

##Die Ressource zu erstellen

Nun, dass die Ressourceneigenschaften erstellt wurden, rufen wir die **neu xDscResource** Cmdlet, um die Ressource zu erstellen. Die **neu xDscResource** Cmdlet nimmt die Liste der Eigenschaften als Parameter. Lässt sich der Pfad, in dem das Modul erstellt werden soll, den Namen der neuen Ressource und der Name des Moduls, in dem es enthalten ist. Der folgende PowerShell-Befehl wird die Ressource erstellt.

```powershell
PS C:\> New-xDscResource –Name Demo_ADUser –Property $UserName, $Ensure, $DomainCredential, $Password –Path ‘C:\Program Files\WindowsPowerShell\Modules’ –ModuleName Demo_DSCModule
```

Die **neu xDscResource** -Cmdlet erstellt, das MOF-Schema, ein Gerüst Ressourcenskript, das erforderliche Verzeichnisstruktur für die neue Ressource und ein Manifest für das Modul, das die neue Ressource verfügbar macht.

Die MOF-Schemadatei befindet sich am **c:\Programme\Microsoft Files\WindowsPowerShell\Modules\Demo_DSCModule\DSCResources\Demo_ADUser\Demo_ADUser.schema.mof**, und sein Inhalt ist wie folgt.

```
[ClassVersion("1.0.0.0"), FriendlyName("Demo_ADUser")]
class Demo_ADUser : OMI_BaseResource
{
[Key] string UserName;
[Write, ValueMap{"Present","Absent"}, Values{"Present","Absent"}] string Ensure;
[Write, EmbeddedInstance("MSFT_Credential")] String DomainCredential;
[Write, EmbeddedInstance("MSFT_Credential")] String Password;
};
```

Das Ressourcenskript befindet sich am **c:\Programme\Microsoft Files\WindowsPowerShell\Modules\Demo_DSCModule\DSCResources\Demo_ADUser\Demo_ADUser.psm1**. Es umfasst nicht die eigentliche Logik, um die Ressource zu implementieren, die Sie selbst hinzufügen müssen. Der Inhalt des Skripts Skelett ist wie folgt.

```powershell
function Get-TargetResource
{
[CmdletBinding()]
[OutputType([System.Collections.Hashtable])]
param
(
[parameter(Mandatory = $true)]
[System.String]
$UserName
)

#Write-Verbose "Use this cmdlet to deliver information about command processing."

#Write-Debug "Use this cmdlet to write debug information while troubleshooting."


<#
$returnValue = @{
UserName = [System.String]
Ensure = [System.String]
DomainAdminCredential = [System.Management.Automation.PSCredential]
Password = [System.Management.Automation.PSCredential]
}

$returnValue
#>
}


function Set-TargetResource
{
[CmdletBinding()]
param
(
[parameter(Mandatory = $true)]
[System.String]
$UserName,

[ValidateSet("Present","Absent")]
[System.String]
$Ensure,

[System.Management.Automation.PSCredential]
$DomainAdminCredential,

[System.Management.Automation.PSCredential]
$Password
)

#Write-Verbose "Use this cmdlet to deliver information about command processing."

#Write-Debug "Use this cmdlet to write debug information while troubleshooting."

#Include this line if the resource requires a system reboot.
#$global:DSCMachineStatus = 1


}


function Test-TargetResource
{
[CmdletBinding()]
[OutputType([System.Boolean])]
param
(
[parameter(Mandatory = $true)]
[System.String]
$UserName,

[ValidateSet("Present","Absent")]
[System.String]
$Ensure,

[System.Management.Automation.PSCredential]
$DomainAdminCredential,

[System.Management.Automation.PSCredential]
$Password
)

#Write-Verbose "Use this cmdlet to deliver information about command processing."

#Write-Debug "Use this cmdlet to write debug information while troubleshooting."


<#
$result = [System.Boolean]

$result
#>
}


Export-ModuleMember -Function *-TargetResource
```

##Aktualisieren der Ressource

Sie müssen zum Hinzufügen oder Ändern der Parameterliste der Ressource, können Sie Aufrufen der **Update xDscResource** Cmdlet. Das Cmdlet aktualisiert die Ressource mit einer neuen Parameterliste. Wenn Sie bereits in Ihr Ressourcenskript Logik hinzugefügt haben, wird die intakt gelassen.

Angenommen Sie, dass Sie das letzte Protokoll Zeit für den Benutzer in unsere Ressource einschließen möchten. Anstatt die Ressource erneut vollständig zu schreiben, rufen Sie die **neu xDscResourceProperty** die neue Eigenschaft zu erstellen, und rufen Sie anschließend **Update xDscResource** und die Liste der Eigenschaften die neue Eigenschaft hinzuzufügen.

```powershell
PS C:\> $lastLogon = New-xDscResourceProperty –Name LastLogon –Type Hashtable –Attribute Write –Description “For mapping users to their last log on time”
PS C:\> Update-xDscResource –Name ‘Demo_ADUser’ –Property $UserName, $Ensure, $DomainCredential, $Password, $lastLogon -Force
```

##Testen ein Ressourcenschema

Mit dem Ressourcen-Designer stellt eine weitere Cmdlet, das verwendet werden kann, um die Gültigkeit des MOF-Schema zu testen, die Sie manuell erstellt haben. Rufen Sie die **Test xDscSchema** -Cmdlet der Pfad des Schemas eine MOF-Ressource als Parameter übergeben. Mit dem Cmdlet werden alle Fehler im Schema ausgegeben.

###Weitere Informationen

####Konzepte

[Erstellen Sie benutzerdefinierter Windows PowerShell gewünscht State Configuration-Ressourcen](authoringResource.md)

####Weitere Ressourcen

[xDscResourceDesigner-Modul](https://powershellgallery.com/packages/xDscResourceDesigner)




