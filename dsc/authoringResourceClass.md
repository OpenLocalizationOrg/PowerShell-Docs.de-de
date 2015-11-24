#Schreiben eine benutzerdefinierte DSC-Ressource mit PowerShell-Klassen

> Gilt für: Windows WindowsPowerShell 5.0

Mit der Einführung von PowerShell-Klassen in Windows PowerShell 5.0 können Sie jetzt eine Ressource DSC durch Erstellen einer Klasse definieren. Die Klasse definiert das Schema und die Implementierung der Ressource, daher keine Notwendigkeit besteht, eine separate MOF-Datei erstellen. Die Ordnerstruktur für eine Ressource auf Klassen basierende wurde vereinfacht, da ein **DSCResources** Ordner ist nicht erforderlich.

In einer klassenbasierten DSC-Ressource ist das Schema als Eigenschaften der Klasse definiert, die mit Attributen, geben Sie die Eigenschaft geändert werden kann. Die Ressource wird implementiert, indem **Get()**, **DISTINCT**, und **Test()** Methoden (entspricht der **Get-TargetResource**, **Set TargetResource**, und **Test TargetResource** Funktionen in einer Skriptressource.

In diesem Thema erstellen wir eine einfache Ressource namens **FileResource** eine Datei in einem angegebenen Pfad verwaltet.

Weitere Informationen zu DSC-Ressourcen finden Sie unter [benutzerdefinierte Windows PowerShell gewünschten Status Konfiguration Buildressourcen](authoringResource.md)

##Ordnerstruktur für eine Klassenressource

Um eine benutzerdefinierte DSC-Ressource mit einer PowerShell-Klasse zu implementieren, erstellen Sie die folgende Ordnerstruktur. Die Klasse definiert ist, **MyDscResource.psm1** und das modulmanifest definiert ist, **MyDscResource.psd1**.

```
$env: psmodulepath (folder)
    |- MyDscResources (folder)
        |- MyDscResource.psm1 
           MyDscResource.psd1 
```

##Erstellen Sie die Klasse

Sie verwenden das Schlüsselwort Class zum Erstellen einer PowerShell-Klasse. Verwenden, um anzugeben, dass eine Klasse eine DSC-Ressource der **DscResource()** Attribut. Der Name der Klasse ist der Name der Ressource DSC.

```powershell
[DscResource()]
class FileResource{
}
```

###Deklarieren von Eigenschaften

Das Schema der DSC Resource wird als Eigenschaften der Klasse definiert. Wir haben drei Eigenschaften wie folgt deklarieren.

```powershell
[DscProperty(Key)]
[string]$Path


[DscProperty(Mandatory)]
[Ensure] $Ensure


[DscProperty(Mandatory)]
[string] $SourcePath

[DscProperty(NotConfigurable)]
[Nullable[datetime]] $CreationTime
```

Beachten Sie, dass die Eigenschaften von Attribute geändert werden. Die Attribues-Zuordnung zum entsprechenden Attribute wie folgt in MOF-Klassen verwendet.

Property-Attribut im MOF-Klassenattribut Beschreibung
DscProperty(Key)
Schlüssel
Die Eigenschaft ist erforderlich. Die Eigenschaft ist ein Schlüssel. Die Werte aller Eigenschaften als Schlüssel zur eindeutigen Identifizierung eine Ressourceninstanz innerhalb einer Konfiguration kombinieren müssen markiert.
DscProperty(Mandatory)
Erforderlich
Die Eigenschaft ist erforderlich.
DscProperty(NotConfigurable)
Lesen
Die Eigenschaft ist schreibgeschützt. Eigenschaften, die mit diesem Attribut markierte können nicht von einer Konfiguration festgelegt werden, aber aufgefüllt werden, von der Get()-Methode, sofern diese vorhanden sind.
DscProperty()
Schreiben
Die Eigenschaft konfigurierbar ist, aber es ist nicht erforderlich.

Die **$Path** und **$SourcePath** Eigenschaften sind beide Zeichenfolgen. Die **$CreationTime** ist ein [DateTime](https://technet.microsoft.com/en-us/library/system.datetime.aspx) Eigenschaft. Die **$Ensure** -Eigenschaft ist ein Enumerationstyp, der wie folgt definiert.

```powershell
enum Ensure 
{ 
    Absent 
    Present 
}
```

###Implementieren die Methoden

Die **Get()**, **DISTINCT**, und **Test()** Methoden sind analog zu den **Get-TargetResource**, **Set TargetResource**, und **Test TargetResource** Funktionen in eine Skriptressource.

Dieser Code umfasst auch die CopyFile()"-Funktion eine Hilfsfunktion, die die Datei kopiert **$SourcePath** auf **$Path**.

```powershell
<#
        This method is equivalent of the Set-TargetResource script function.
        It sets the resource to the desired state.
    #>
    [void] Set()
    {        
        $fileExists = $this.TestFilePath($this.Path)
        if($this.ensure -eq [Ensure]::Present)
        {
            if(-not $fileExists)
            {
                $this.CopyFile()
            }
        }
        else
        {
            if($fileExists)
            {
                Write-Verbose -Message "Deleting the file $($this.Path)"
                Remove-Item -LiteralPath $this.Path -Force
            }
        }
    }

    <# 

        This method is equivalent of the Test-TargetResource script function. 
        It should return True or False, showing whether the resource
        is in a desired state.         
    #>
    [bool] Test()
    {
        $present = $this.TestFilePath($this.Path)

        if($this.Ensure -eq [Ensure]::Present)
        {
            return $present
        }
        else
        {
            return -not $present
        }
    }


    <#
        This method is equivalent of the Get-TargetResource script function.
        The implementation should use the keys to find appropriate resources.
        This method returns an instance of this class with the updated key
         properties.
    #>
    [FileResource] Get()
    {
        $present = $this.TestFilePath($this.Path)        

        if ($present)
        {
            $file = Get-ChildItem -LiteralPath $this.Path
            $this.CreationTime = $file.CreationTime
            $this.Ensure = [Ensure]::Present
        }
        else
        {
            $this.CreationTime = $null
            $this.Ensure = [Ensure]::Absent
        }        

        return $this
    }

    <#
        Helper method to check if the file exists and it is file
    #>
    [bool] TestFilePath([string] $location)
    {      
        $present = $true

        $item = Get-ChildItem -LiteralPath $location -ea Ignore
        if ($item -eq $null)
        {
            $present = $false            
        }
        elseif( $item.PSProvider.Name -ne "FileSystem")
        {
            throw "Path $($location) is not a file path."
        }
        elseif($item.PSIsContainer)
        {
            throw "Path $($location) is a directory path."
        }
        return $present
    }

    <#
        Helper method to copy file from source to path
    #>
    [void] CopyFile()
    { 
        if(-not $this.TestFilePath($this.SourcePath))
        {
            throw "SourcePath $($this.SourcePath) is not found."
        }

        [System.IO.FileInfo] $destFileInfo = new-object System.IO.FileInfo($this.Path)
        if (-not $destFileInfo.Directory.Exists)
        {
            Write-Verbose -Message "Creating directory $($destFileInfo.Directory.FullName)"

            #use CreateDirectory instead of New-Item to avoid code
            # to handle the non-terminating error
            [System.IO.Directory]::CreateDirectory($destFileInfo.Directory.FullName)
        }

        if(Test-Path -LiteralPath $this.Path -PathType Container)
        {
            throw "Path $($this.Path) is a directory path"
        }

        Write-Verbose -Message "Copying $($this.SourcePath) to $($this.Path)"

        #DSC engine catches and reports any error that occurs
        Copy-Item -LiteralPath $this.SourcePath -Destination $this.Path -Force
    }
}<# 
        This method replaces the Set-TargetResource DSC script function.
        It sets the resource to the desired state. 
    #>
    [void] Set() 
    {       
        $fileExists = Test-Path -path $this.Path -PathType Leaf 
        if($this.ensure -eq [Ensure]::Present)
        {
            if(-not $fileExists)
            {
                $this.CopyFile()            
            }
        }
        else
        {
            if($fileExists)         
            {
                Write-Verbose -Message "Deleting the file $this.Path"
                Remove-Item -LiteralPath $this.Path                     
            }
        } 
    } 


    <# 

        This method replaces the Test-TargetResource function. 
        It should return True or False, showing whether the resource
         is in a desired state.         
    #> 

    [bool] Test()
    {
        if(Test-Path -path $this.Path -PathType Container)
        {
            throw "Path '$this.Path' is a directory path."
        }

        $fileExists = Test-Path -path $this.Path -PathType Leaf

        if($this.ensure -eq [Ensure]::Present)
        {
            return $fileExists
        }

        return (-not $fileExists)
    }


    <# 
        This method replaces the Get-TargetResource function.         
        The implementation should use the keys to find appropriate resources. 
        This method returns an instance of this class with the updated key properties. 
    #> 

    [FileResource] Get() 
    {
        $file = Get-item $this.Path
        return $this
    }

    <#
        Helper method to copy file from source to path.
        Because this resource provider run under system,
        Only the Administrators and system have full
         access to the new created directory and file
    #>
    CopyFile()
    {
        if(Test-Path -path $this.SourcePath -PathType Container)
        {
            throw "SourcePath '$this.SourcePath' is a directory path"
        }

        if( -not (Test-Path -path $this.SourcePath -PathType Leaf))
        {
            throw "SourcePath '$this.SourcePath' is not found."
        }

        [System.IO.FileInfo] $destFileInfo = new-object System.IO.FileInfo($this.Path)
        if (-not $destFileInfo.Directory.Exists)
        {         
            Write-Verbose -Message "Creating directory $($destFileInfo.Directory.FullName)"

            #use CreateDirectory instead of New-Item to avoid lines
            # to handle the non-terminating error
            [System.IO.Directory]::CreateDirectory($destFileInfo.Directory.FullName)
        }

        if(Test-Path -path $this.Path -PathType Container)
        {
            throw "Path '$this.Path' is a directory path"
        }

        Write-Verbose -Message "Copying $this.SourcePath to $this.Path"

        #DSC engine catches and reports any error that occurs
        Copy-Item -Path $this.SourcePath -Destination $this.Path -Force
    } 
 <# 
        This method replaces the Set-TargetResource DSC script function.
        It sets the resource to the desired state. 
    #>
    [void] Set() 
    {       
        $fileExists = Test-Path -path $Path -PathType Leaf 
        if($ensure -eq [Ensure]::Present)
        {
            if(-not $fileExists)
            {
                $this.CopyFile()            
            }
        }
        else
        {
            if($fileExists)         
            {
                Write-Verbose -Message "Deleting the file $Path"
                Remove-Item -LiteralPath $Path                     
            }
        } 
    } 


    <# 

        This method replaces the Test-TargetResource function. 
        It should return True or False, showing whether the resource
         is in a desired state.         
    #> 

    [bool] Test()
    {
        if(Test-Path -path $Path -PathType Container)
        {
            throw "Path '$Path' is a directory path."
        }

        $fileExists = Test-Path -path $Path -PathType Leaf
if($ensure -eq [Ensure]::Present)
        {
            return $fileExists
        }

        return (-not $fileExists)
    }


    <# 
        This method replaces the Get-TargetResource function.         
        The implementation should use the keys to find appropriate resources. 
        This method returns an instance of this class with the updated key properties. 
    #> 

    [FileResource] Get() 
    {
        $file = Get-item $Path
        return $this
    }

    <#
        Helper method to copy file from source to path
        Because this resource provider run under system,
        Only the Administrators and system have full
         access to the new created directory and file
    #>
    CopyFile()
    {
        if(Test-Path -path $SourcePath -PathType Container)
        {
            throw "SourcePath '$SourcePath' is a directory path"
        }

        if( -not (Test-Path -path $SourcePath -PathType Leaf))
        {
            throw "SourcePath '$SourcePath' is not found."
        }

        [System.IO.FileInfo] $destFileInfo = new-object System.IO.FileInfo($Path)
        if (-not $destFileInfo.Directory.Exists)
        {         
            Write-Verbose -Message "Creating directory $($destFileInfo.Directory.FullName)"

            #use CreateDirectory instead of New-Item to avoid lines
            # to handle the non-terminating error
            [System.IO.Directory]::CreateDirectory($destFileInfo.Directory.FullName)
        }

        if(Test-Path -path $Path -PathType Container)
        {
            throw "Path '$Path' is a directory path"
        }

        Write-Verbose -Message "Copying $SourcePath to $Path"

        #DSC engine catches and reports any error that occurs
        Copy-Item -Path $SourcePath -Destination $Path -Force
    }
```

###Die vollständige Datei

Die vollständige Klasse-Datei:

```powershell
enum Ensure
{
    Absent
    Present
}

<#
   This resource manages the file in a specific path.
   [DscResource()] indicates the class is a DSC resource
#>

[DscResource()]
class FileResource
{
    <# 
       This property is the fully qualified path to the file that is
       expected to be present or absent.

       The [DscProperty(Key)] attribute indicates the property is a
       key and its value uniquely identifies a resource instance.
       Defining this attribute also means the property is required
       and DSC will ensure a value is set before calling the resource.

       A DSC resource must define at least one key property.
    #>
    [DscProperty(Key)]
    [string]$Path

    <#
        This property indicates if the settings should be present or absent
        on the system. For present, the resource ensures the file pointed
        to by $Path exists. For absent, it ensures the file point to by
        $Path does not exist.

        The [DscProperty(Mandatory)] attribute indicates the property is
        required and DSC will guarantee it is set.

        If Mandatory is not specified or if it is defined as 
        Mandatory=$false, the value is not guaranteed to be set when DSC
        calls the resource.  This is appropriate for optional properties.
    #>
    [DscProperty(Mandatory)]
    [Ensure] $Ensure    

    <#
       This property defines the fully qualified path to a file that will
       be placed on the system if $Ensure = Present and $Path does not
        exist.

       NOTE: This property is required because [DscProperty(Mandatory)] is
        set.
    #>
    [DscProperty(Mandatory)]
    [string] $SourcePath

    <#
       This property reports the file's create timestamp.

       [DscProperty(NotConfigurable)] attribute indicates the property is
       not configurable in DSC configuration.  Properties marked this way
       are populated by the Get() method to report additional details
       about the resource when it is present.

    #>
    [DscProperty(NotConfigurable)]
    [Nullable[datetime]] $CreationTime

    <#
        This method is equivalent of the Set-TargetResource script function.
        It sets the resource to the desired state.
    #>
    [void] Set()
    {        
        $fileExists = $this.TestFilePath($this.Path)
        if($this.ensure -eq [Ensure]::Present)
        {
            if(-not $fileExists)
            {
                $this.CopyFile()
            }
        }
        else
        {
            if($fileExists)
            {
                Write-Verbose -Message "Deleting the file $($this.Path)"
                Remove-Item -LiteralPath $this.Path -Force
            }
        }
    }

    <# 

        This method is equivalent of the Test-TargetResource script function. 
        It should return True or False, showing whether the resource
        is in a desired state.         
    #>
    [bool] Test()
    {
        $present = $this.TestFilePath($this.Path)

        if($this.Ensure -eq [Ensure]::Present)
        {
            return $present
        }
        else
        {
            return -not $present
        }
    }


    <#
        This method is equivalent of the Get-TargetResource script function.
        The implementation should use the keys to find appropriate resources.
        This method returns an instance of this class with the updated key
         properties.
    #>
    [FileResource] Get()
    {
        $present = $this.TestFilePath($this.Path)        

        if ($present)
        {
            $file = Get-ChildItem -LiteralPath $this.Path
            $this.CreationTime = $file.CreationTime
            $this.Ensure = [Ensure]::Present
        }
        else
        {
            $this.CreationTime = $null
            $this.Ensure = [Ensure]::Absent
        }        

        return $this
    }

    <#
        Helper method to check if the file exists and it is file
    #>
    [bool] TestFilePath([string] $location)
    {      
        $present = $true

        $item = Get-ChildItem -LiteralPath $location -ea Ignore
        if ($item -eq $null)
        {
            $present = $false            
        }
        elseif( $item.PSProvider.Name -ne "FileSystem")
        {
            throw "Path $($location) is not a file path."
        }
        elseif($item.PSIsContainer)
        {
            throw "Path $($location) is a directory path."
        }
        return $present
    }

    <#
        Helper method to copy file from source to path
    #>
    [void] CopyFile()
    { 
        if(-not $this.TestFilePath($this.SourcePath))
        {
            throw "SourcePath $($this.SourcePath) is not found."
        }

        [System.IO.FileInfo] $destFileInfo = new-object System.IO.FileInfo($this.Path)
        if (-not $destFileInfo.Directory.Exists)
        {
            Write-Verbose -Message "Creating directory $($destFileInfo.Directory.FullName)"

            #use CreateDirectory instead of New-Item to avoid code
            # to handle the non-terminating error
            [System.IO.Directory]::CreateDirectory($destFileInfo.Directory.FullName)
        }

        if(Test-Path -LiteralPath $this.Path -PathType Container)
        {
            throw "Path $($this.Path) is a directory path"
        }

        Write-Verbose -Message "Copying $($this.SourcePath) to $($this.Path)"

        #DSC engine catches and reports any error that occurs
        Copy-Item -LiteralPath $this.SourcePath -Destination $this.Path -Force
    }
}# This module defines a class for a DSC "FileResource" provider. 


enum Ensure 
{ 
    Absent 
    Present 
} 


<# This resource manages the file in a specific path. 
[DscResource()] indicates the class is a DSC resource 
#> 

[DscResource()]
class FileResource{ 

    <# This is a key property 
       [DscResourceKey()] also means the property is required.
       It is guaranteed to be set, other properties may not
        be set if the configuration did not specify values.
    #> 
    [DscResourceKey()]
    [string]$Path

    <# 
       [DscResourceMandatory()] means the property is required.
       It is guaranteed to be set, other properties may not be set
        if the configuration did not specify values.
    #>
    [DscResourceMandatory()]
    [Ensure] $Ensure

    <# 
       [DscResourceMandatory()] means the property is required.
    #>
    [DscResourceMandatory()]
    [string] $SourcePath     

    [DscResource

    <# 
        This method replaces the Set-TargetResource DSC script function.
        It sets the resource to the desired state. 
    #>
    [void] Set() 
    {       
        $fileExists = Test-Path -path $this.Path -PathType Leaf 
        if($this.ensure -eq [Ensure]::Present)
        {
            if(-not $fileExists)
            {
                $this.CopyFile()            
            }
        }
        else
        {
            if($fileExists)         
            {
                Write-Verbose -Message "Deleting the file $this.Path"
                Remove-Item -LiteralPath $this.Path                     
            }
        } 
    } 


    <# 

        This method replaces the Test-TargetResource function. 
        It should return True or False, showing whether the resource
         is in a desired state.         
    #> 

    [bool] Test()
    {
        if(Test-Path -path $this.Path -PathType Container)
        {
            throw "Path '$this.Path' is a directory path."
        }

        $fileExists = Test-Path -path $this.Path -PathType Leaf

        if($this.ensure -eq [Ensure]::Present)
        {
            return $fileExists
        }

        return (-not $fileExists)
    }


    <# 
        This method replaces the Get-TargetResource function.         
        The implementation should use the keys to find appropriate resources. 
        This method returns an instance of this class with the updated key properties. 
    #> 

    [FileResource] Get() 
    {
        $file = Get-item $this.Path
        return $this
    }

    <#
        Helper method to copy file from source to path.
        Because this resource provider run under system,
        Only the Administrators and system have full
         access to the new created directory and file
    #>
    CopyFile()
    {
        if(Test-Path -path $this.SourcePath -PathType Container)
        {
            throw "SourcePath '$this.SourcePath' is a directory path"
        }

        if( -not (Test-Path -path $this.SourcePath -PathType Leaf))
        {
            throw "SourcePath '$this.SourcePath' is not found."
        }

        [System.IO.FileInfo] $destFileInfo = new-object System.IO.FileInfo($this.Path)
        if (-not $destFileInfo.Directory.Exists)
        {         
            Write-Verbose -Message "Creating directory $($destFileInfo.Directory.FullName)"

            #use CreateDirectory instead of New-Item to avoid lines
            # to handle the non-terminating error
            [System.IO.Directory]::CreateDirectory($destFileInfo.Directory.FullName)
        }

        if(Test-Path -path $this.Path -PathType Container)
        {
            throw "Path '$this.Path' is a directory path"
        }

        Write-Verbose -Message "Copying $this.SourcePath to $this.Path"

        #DSC engine catches and reports any error that occurs
        Copy-Item -Path $this.SourcePath -Destination $this.Path -Force
    }
} 
```

##Erstellen Sie eine Manifestdatei

Um eine klassenbasierte Ressource dem DSC-Modul verfügbar zu machen, müssen Sie enthalten eine **DscResourcesToExport** Anweisung in der Manifestdatei, die weist das Modul, um die Ressource zu exportieren. Die Manifestdatei sieht folgendermaßen aus:

```powershell
@{

# Script module or binary module file associated with this manifest.
RootModule = 'MyDscResource.psm1'

DscResourcesToExport = 'FileResource'

# Version number of this module.
ModuleVersion = '1.0'

# ID used to uniquely identify this module
GUID = '81624038-5e71-40f8-8905-b1a87afe22d7'

# Author of this module
Author = 'Microsoft Corporation'

# Company or vendor of this module
CompanyName = 'Microsoft Corporation'

# Copyright statement for this module
Copyright = '(c) 2014 Microsoft. All rights reserved.'

# Description of the functionality provided by this module
# Description = ''

# Minimum version of the Windows PowerShell engine required by this module
PowerShellVersion = '5.0'

# Name of the Windows PowerShell host required by this module
# PowerShellHostName = ''
} 
```

##Testen Sie die Ressource

Nach dem Speichern der Klasse und manifest-Dateien in der Ordnerstruktur wie oben beschrieben, können Sie eine Konfiguration erstellen, die die neue Ressource verwendet. Informationen über das Ausführen einer DSC-Konfigurations finden Sie unter [Inkraftsetzung Konfigurationen](enactingConfigurations.md). Die folgende Konfiguration wird überprüft, ob die Datei unter `c:\test\test.txt` vorhanden ist, und falls nicht, kopiert die Datei vom `c:\test.txt` (erstellen Sie `c:\test.txt` vor dem Ausführen der Konfigurations).

```powershell
Configuration Test
{
    Import-DSCResource -module MyDscResource
    FileResource file
    {
        Path = "C:\test\test.txt"
        SourcePath = "c:\test.txt"
        Ensure = "Present"
    } 
}
Test
Start-DscConfiguration -Wait -Force Test
```

##Weitere Informationen

###Konzepte

[Erstellen Sie benutzerdefinierter Windows PowerShell gewünscht State Configuration-Ressourcen](authoringResource.md)



