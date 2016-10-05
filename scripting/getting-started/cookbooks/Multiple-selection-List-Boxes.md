---
title: "Listenfelder für Mehrfachauswahl"
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: f74cd5d9-da57-4802-b614-0b194a7bc8f8
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 1adae567893fb5dfcf5b5f675ca587a1d437d39a

---

# Listenfelder für Mehrfachauswahl
Verwenden Sie Windows PowerShell 3.0 und höhere Versionen, um ein Listenfeld-Steuerelement für die Mehrfachauswahl in einem benutzerdefinierten Windows-Formular zu erstellen.

## Erstellen von Listenfeld-Steuerelementen, die Mehrfachauswahl unterstützen
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
$label.Text = "Please make a selection from the list below:"
$form.Controls.Add($label) 

$listBox = New-Object System.Windows.Forms.Listbox 
$listBox.Location = New-Object System.Drawing.Point(10,40) 
$listBox.Size = New-Object System.Drawing.Size(260,20) 

$listBox.SelectionMode = "MultiExtended"

[void] $listBox.Items.Add("Item 1")
[void] $listBox.Items.Add("Item 2")
[void] $listBox.Items.Add("Item 3")
[void] $listBox.Items.Add("Item 4")
[void] $listBox.Items.Add("Item 5")

$listBox.Height = 70
$form.Controls.Add($listBox) 
$form.Topmost = $True

$result = $form.ShowDialog()

if ($result -eq [System.Windows.Forms.DialogResult]::OK)
{
    $x = $listBox.SelectedItems
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
$OKButton.Location = New-Object System.Drawing.Size(75,120)
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
$label.Text = "Please make a selection from the list below:"
$form.Controls.Add($label)
```

Fügen Sie das Steuerelement (in diesem Fall ein Listenfeld) hinzu, mit dem Benutzer die Informationen bereitstellen, die Sie in Ihrem Beschriftungstext beschrieben haben. Es gibt viele weitere Steuerelemente, die Sie neben Textfeldern anwenden können. Weitere Steuerelemente finden Sie unter [System.Windows.Forms Namespace](http://msdn.microsoft.com/library/k50ex0x9(v=vs.110).aspx) auf MSDN.

```
$listBox = New-Object System.Windows.Forms.Listbox 
$listBox.Location = New-Object System.Drawing.Point(10,40) 
$listBox.Size = New-Object System.Drawing.Size(260,20)
```

Auf folgende Weise können Sie angeben, dass Sie Benutzern die Auswahl mehrerer Werte in der Liste erlauben möchten.

```
$listBox.SelectionMode = "MultiExtended"
```

Im nächsten Abschnitt legen Sie die Werte fest, die im Listenfeld für Benutzer angezeigt werden sollen.

```
[void] $listBox.Items.Add("Item 1")
[void] $listBox.Items.Add("Item 2")
[void] $listBox.Items.Add("Item 3")
[void] $listBox.Items.Add("Item 4")
[void] $listBox.Items.Add("Item 5")
```

Geben Sie die maximale Höhe des Listenfeld-Steuerelements an.

```
$listBox.Height = 70
```

Fügen Sie das Listenfeld-Steuerelement zu Ihrem Formular hinzu, um Windows anzuweisen, das Formular beim Öffnen über anderen Fenstern und Dialogfeldern zu öffnen.

```
$form.Controls.Add($listBox) 
$form.Topmost = $True
```

Fügen Sie die folgende Codezeile hinzu, um das Formular in Windows anzuzeigen.

```
$result = $form.ShowDialog()
```

Schließlich weist der Code im **If**-Block Windows an, was mit dem Formular geschehen soll, wenn Benutzer eine oder mehrere Optionen im Listenfeld auswählen und dann auf die Schaltfläche **OK** klicken oder die **EINGABETASTE** drücken.

```
if ($result -eq [System.Windows.Forms.DialogResult]::OK)
{
    $x = $listBox.SelectedItems
    $x
}
```

## Weitere Informationen
[Hey Scripting Guy: Warum funktionieren diese PowerShell GUI-Beispiele nicht?](http://go.microsoft.com/fwlink/?LinkId=506644)
[GitHub: WinFormsExampleUpdates von Dave Wyatt](https://github.com/dlwyatt/WinFormsExampleUpdates)
[Windows PowerShell Tip of the Week: Multi-Select List Boxes – And More!](http://technet.microsoft.com/library/ff730950.aspx)




<!--HONumber=Oct16_HO1-->


