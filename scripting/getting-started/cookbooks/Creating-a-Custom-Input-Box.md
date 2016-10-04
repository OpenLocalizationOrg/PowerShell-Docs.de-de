---
title: Erstellen eines benutzerdefinierten Eingabefelds
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 0b12e56c-299f-40ee-afbf-d30d23ed2565
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 6c0c1d42dae602a83bbad7a0abe8df1b6435489a

---

# Erstellen eines benutzerdefinierten Eingabefelds
Schreiben Sie ein benutzerdefiniertes graphisches Eingabefeld mit Microsoft .NET Framework-Formularerstellungsfunktionen in Windows PowerShell 3.0 und späteren Versionen.

## Erstellen Sie eine benutzerdefiniertes, graphisches Eingabefeld
Kopieren und fügen Sie Folgendes in Windows PowerShell ISE ein, und speichern Sie es als Windows PowerShell-Skript (.ps1).

```
Add-Type -AssemblyName System.Windows.Forms
Add-Type -AssemblyName System.Drawing

$form = New-Object System.Windows.Forms.Form 
$form.Text = "Data Entry Form"
$form.Size = New-Object System.Drawing.Size(300,200) 
$form.StartPosition = "CenterScreen"

$OKButton = New-Object System.Windows.Forms.Button
$OKButton.Location = New-Object System.Drawing.Point(75,120)
$OKButton.Size = New-Object System.Drawing.Size(75,23)
$OKButton.Text = "OK"
$OKButton.DialogResult = [System.Windows.Forms.DialogResult]::OK
$form.AcceptButton = $OKButton
$form.Controls.Add($OKButton)

$CancelButton = New-Object System.Windows.Forms.Button
$CancelButton.Location = New-Object System.Drawing.Point(150,120)
$CancelButton.Size = New-Object System.Drawing.Size(75,23)
$CancelButton.Text = "Cancel"
$CancelButton.DialogResult = [System.Windows.Forms.DialogResult]::Cancel
$form.CancelButton = $CancelButton
$form.Controls.Add($CancelButton)

$label = New-Object System.Windows.Forms.Label
$label.Location = New-Object System.Drawing.Point(10,20) 
$label.Size = New-Object System.Drawing.Size(280,20) 
$label.Text = "Please enter the information in the space below:"
$form.Controls.Add($label) 

$textBox = New-Object System.Windows.Forms.TextBox 
$textBox.Location = New-Object System.Drawing.Point(10,40) 
$textBox.Size = New-Object System.Drawing.Size(260,20) 
$form.Controls.Add($textBox) 

$form.Topmost = $True

$form.Add_Shown({$textBox.Select()})
$result = $form.ShowDialog()

if ($result -eq [System.Windows.Forms.DialogResult]::OK)
{
    $x = $textBox.Text
    $x
}
```

Das Skript beginnt mit dem Laden von zwei .NET Framework-Klassen: **System.Drawing** und **System.Windows.Forms**. Sie starten daraufhin eine neue Instanz der .NET Framework-Klasse **System.Windows.Forms.Form**, die ein leeres Formular oder Fenster bereitstellt, zu dem Sie Steuerelemente hinzufügen können.

```
$form = New-Object System.Windows.Forms.Form
```

Nachdem Sie eine Instanz der Formularklasse erstellt haben, ordnen Sie drei Eigenschaften dieser Klasse Werte zu.

-   **Text.** Dies wird der Titel des Fensters.

-   **Größe.** Dies ist die Größe des Formulars, in Pixeln. Das vorhergehende Skript erstellt ein Formular, das 300 Pixel breit und 200 Pixel hoch ist.

-   **StartingPosition.** Für diese optionale Eigenschaft ist im Skript oben **CenterScreen** festgelegt. Wenn Sie diese Eigenschaft nicht hinzufügen, wählt Windows eine Stelle aus, wenn das Formular geöffnet wird. Durch Festlegen der **StartingPosition** auf **CenterScreen** wird das Formular automatisch bei jedem Laden in der Mitte des Bildschirms angezeigt.

```
$form.Text = "Data Entry Form"
$form.Size = New-Object System.Drawing.Size(300,200) 
$form.StartPosition = "CenterScreen"
```

Als Nächstes erstellen Sie eine Schaltfläche **OK** für Ihr Formular. Legen Sie die Größe und das Verhalten der Schaltfläche **OK** fest. In diesem Beispiel befindet sich die Schaltfläche 120 Pixel vom oberen Formularrand und 75 Pixel vom linken Rand entfernt. Die Schaltflächenhöhe beträgt 23 Pixel und die Schaltflächenlänge 75 Pixel. Das Skript verwendet vordefinierte Windows-Formulartypen zur Bestimmung des Schaltflächenverhaltens.

```
$OKButton = New-Object System.Windows.Forms.Button
$OKButton.Location = New-Object System.Drawing.Point(75,120)
$OKButton.Size = New-Object System.Drawing.Size(75,23)
$OKButton.Text = "OK"
$OKButton.DialogResult = [System.Windows.Forms.DialogResult]::OK
$form.AcceptButton = $OKButton
$form.Controls.Add($OKButton)
```

In entsprechender Weise erstellen Sie eine Schaltfläche **Abbrechen**. Die **Abbrechen**-Schaltfläche ist 120 Pixel vom oberen und 150 Pixel vom linken Rand des Fensters entfernt.

```
$CancelButton = New-Object System.Windows.Forms.Button
$CancelButton.Location = New-Object System.Drawing.Point(150,120)
$CancelButton.Size = New-Object System.Drawing.Size(75,23)
$CancelButton.Text = "Cancel"
$CancelButton.DialogResult = [System.Windows.Forms.DialogResult]::Cancel
$form.CancelButton = $CancelButton
$form.Controls.Add($CancelButton)
```

Als nächstes stellen Sie einen Beschriftungstext in Ihrem Fenster bereit, der die Information beschreibt, die Benutzer verwenden sollen.

```
$label = New-Object System.Windows.Forms.Label
$label.Location = New-Object System.Drawing.Point(10,20) 
$label.Size = New-Object System.Drawing.Size(280,20) 
$label.Text = "Please enter the information in the space below:"
$form.Controls.Add($label)
```

Fügen Sie das Steuerelement (in diesem Fall ein Textfeld) hinzu, mit dem Benutzer die Informationen bereitstellen, die Sie in Ihrem Beschriftungstext beschrieben haben. Es gibt viele weitere Steuerelemente, die Sie neben Textfeldern anwenden können. Weitere Steuerelemente finden Sie unter [System.Windows.Forms Namespace](http://msdn.microsoft.com/library/k50ex0x9(v=vs.110).aspx) auf MSDN.

```
$textBox = New-Object System.Windows.Forms.TextBox 
$textBox.Location = New-Object System.Drawing.Point(10,40) 
$textBox.Size = New-Object System.Drawing.Size(260,20) 
$form.Controls.Add($textBox)
```

Legen Sie die Eigenschaft **Topmost** auf **$True** fest, um zu erzwingen, dass das Fenster über anderen geöffneten Fenstern und Dialogfeldern geöffnet wird.

```
$form.Topmost = $True
```

Fügen Sie als nächstes diese Codezeile zum Aktivieren des Formulars hinzu, und stellen Sie den Fokus auf das von Ihnen erstellte Textfeld ein.

```
$form.Add_Shown({$textBox.Select()})
```

Fügen Sie die folgende Codezeile hinzu, um das Formular in Windows anzuzeigen.

```
$result = $form.ShowDialog()
```

Abschließend weist der Code im Block **If** Windows an, was mit dem Formular geschehen soll, wenn Benutzer Test in das Textfeld eingeben und anschließend auf die Schaltfläche **OK** klicken oder die **EINGABETASTE** drücken.

```
if ($result -eq [System.Windows.Forms.DialogResult]::OK)
{
    $x = $textBox.Text
    $x
}
```

## Weitere Informationen
[Hey Scripting Guy: Warum funktionieren diese PowerShell GUI-Beispiele nicht?](http://go.microsoft.com/fwlink/?LinkId=506644)
[GitHub: WinFormsExampleUpdates von Dave Wyatt](https://github.com/dlwyatt/WinFormsExampleUpdates)
[Windows PowerShell Tip of the Week: Erstellen eines benutzerdefinierten Eingabefelds](http://technet.microsoft.com/library/ff730941.aspx)




<!--HONumber=Oct16_HO1-->


