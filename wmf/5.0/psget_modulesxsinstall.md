# Unterstützung der gleichzeitigen Ausführung unterschiedlicher Versionen für PowerShell 5.0 oder höher

Für die in Windows PowerShell 5.0 oder höher ausgeführten Cmdlets „Install-Module“ „Update-Module“ und „Publish-Module“ wird nun die gleichzeitige Ausführung unterschiedlicher Versionen unterstützt.
Außerdem haben wir dem Cmdlet „Publish-Module“ den Parameter „-RequiredVersion“ hinzugefügt, um die Version anzugeben, die veröffentlicht werden soll. Der „Path“-Parameter unterstützt jetzt den Modulbasispfad mit dem Ordner „version“.

**Beispiele für die Install-Module:**
```powershell
PS C:\\windows\\system32&gt; Install-Module -Name PSScriptAnalyzer -RequiredVersion 1.1.0 -Repository PSGallery
PS C:\\windows\\system32&gt; Get-Module -ListAvailable -Name PSScriptAnalyzer | Format-List Name,Version,ModuleBase
Name : PSScriptAnalyzer
Version : 1.1.0
ModuleBase : C:\Program Files\WindowsPowerShell\Modules\PSScriptAnalyzer\1.1.0

PS C:\\windows\\system32&gt; Install-Module -Name PSScriptAnalyzer -RequiredVersion 1.1.1 -Repository PSGallery
PS C:\\windows\\system32&gt; Get-Module -ListAvailable -Name PSScriptAnalyzer | Format-List Name,Version,ModuleBase
Name       : PSScriptAnalyzer 
Version    : 1.1.1
ModuleBase : C:\Program Files\WindowsPowerShell\Modules\PSScriptAnalyzer\1.1.1
Name       : PSScriptAnalyzer
Version    : 1.1.0
ModuleBase : C:\Program Files\WindowsPowerShell\Modules\PSScriptAnalyzer\1.1.0

PS C:\\windows\\system32&gt; Get-InstalledModule -Name PSScriptAnalyzer -AllVersions
Version    Name                                Type       Repository           Description            
-------    ----                                ----       ----------           -----------            
1.1.0      PSScriptAnalyzer                    Module     PSGallery            PSScriptAnalyzer provides script analysis... 
1.1.1      PSScriptAnalyzer                    Module     PSGallery            PSScriptAnalyzer provides script analysis...
```


<!--HONumber=Oct16_HO1-->


