## Wie lässt sich das Problem „WARNUNG: Fehler beim Herunterladen des Pakets ‚Name Ihres Pakets‘“ lösen?




Uns wurde mitgeteilt, dass bei „Install-Module“ oder „Update-Module“ auf einigen Computern Fehler ausgelöst werden.
Unseren Untersuchungen zufolge hängt dieses Problem mit der Netzwerkverbindung zusammen.
Der NuGet-Anbieter wurde vor Kurzem aktualisiert, sodass Pakete nun zuverlässig heruntergeladen werden können.
Befolgen Sie die unten stehenden Anweisungen, um den aktuellen Build des NuGet-Anbieters zu installieren und anschließend Ihr Modul zu installieren oder zu aktualisieren.
Im nachfolgenden Beispiel wird das Modul „Azure“ verwendet.

```powershell
Install-PackageProvider NuGet -MinimumVersion 2.8.5.206 -Force
Launch new PowerShell Console
Update-Module Azure -Verbose
```


<!--HONumber=Oct16_HO1-->


