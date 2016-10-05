# „Clipboard“-Cmdlets
Die Cmdlets **Get-Clipboard** und **Set-Clipboard** vereinfachen das Übertragen von Inhalten in eine und aus einer Windows PowerShell-Sitzung. Wenn Sie beispielsweise Windows-Explorer verwenden, um drei Dateien in die Zwischenablage zu kopieren (z.B. indem Sie sie auswählen und `ctrl-c` drücken), können Sie nun einfach auf den Inhalt der Zwischenablage als Liste von Dateien zugreifen:

```powershell 
PS C:\\&gt; Get-Clipboard -Format FileDropList

Directory: C:\\Users\\slee\\Downloads\\Example

Mode LastWriteTime Length Name

---- ------------- ------ ----

-a---- 4/14/2015 1:19 PM 0 File2.txt

-a---- 4/14/2015 1:19 PM 0 File3.txt

-a---- 4/14/2015 1:19 PM 0 File1.txt
```


Die „Clipboard“-Cmdlets unterstützen Bilder, Audiodateien, Dateilisten und Text.


<!--HONumber=Oct16_HO1-->


