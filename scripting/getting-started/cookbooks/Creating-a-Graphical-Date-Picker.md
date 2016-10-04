---
title: Erstellen einer grafischen Datumsauswahl
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: c1cb722c-41e9-4baa-be83-59b4653222e9
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 9c232f47eba22790dfc98e4948c71e181616d2ae

---

# Erstellen einer grafischen Datumsauswahl
Verwenden Sie Windows PowerShell 3.0 und neuere Versionen, um ein Formular mit einem grafischen Kalendersteuerelement zu erstellen, mit dem Benutzer einen Tag des Monats auswählen können.

## Erstellen eines grafischen Steuerelements für die Datumsauswahl
Kopieren und fügen Sie Folgendes in Windows PowerShell ISE ein, und speichern Sie es als Windows PowerShell-Skript (.ps1).

```
Add-Type -AssemblyName System.Windows.Forms
Add-Type -AssemblyName System.Drawing

$form = New-Object Windows.Forms.Form 

$form.Text = "Select a Date" 
$form.Size = New-Object Drawing.Size @(243,230) 
$form.StartPosition = "CenterScreen"

$calendar = New-Object System.Windows.Forms.MonthCalendar 
$calendar.ShowTodayCircle = $False
$calendar.MaxSelectionCount = 1
$form.Controls.Add($calendar) 

$OKButton = New-Object System.Windows.Forms.Button
$OKButton.Location = New-Object System.Drawing.Point(38,165)
$OKButton.Size = New-Object System.Drawing.Size(75,23)
$OKButton.Text = "OK"
$OKButton.DialogResult = [System.Windows.Forms.DialogResult]::OK
$form.AcceptButton = $OKButton
$form.Controls.Add($OKButton)

$CancelButton = New-Object System.Windows.Forms.Button
$CancelButton.Location = New-Object System.Drawing.Point(113,165)
$CancelButton.Size = New-Object System.Drawing.Size(75,23)
$CancelButton.Text = "Cancel"
$CancelButton.DialogResult = [System.Windows.Forms.DialogResult]::Cancel
$form.CancelButton = $CancelButton
$form.Controls.Add($CancelButton)

$form.Topmost = $True

$result = $form.ShowDialog() 

if ($result -eq [System.Windows.Forms.DialogResult]::OK)
{
    $date = $calendar.SelectionStart
    Write-Host "Date selected: $($date.ToShortDateString())"
}
```

Das Skript beginnt mit dem Laden von zwei .NET Framework-Klassen: **System.Drawing** und **System.Windows.Forms**. Sie starten dann eine neue Instanz der .NET Framework-Klasse **Windows.Forms.Form**. Diese stellt ein leeres Formular oder Fenster bereit, in das Sie Steuerelemente einfügen können.

```
$form = New-Object Windows.Forms.Form
```

Nachdem Sie eine Instanz der Formularklasse erstellt haben, ordnen Sie drei Eigenschaften dieser Klasse Werte zu.

-   **Text.** Dies wird der Titel des Fensters.

-   **Größe.** Dies ist die Größe des Formulars, in Pixeln. Dieses Skript erstellt ein Formular, das 243 Pixel breit und 230 Pixel hoch ist.

-   **StartingPosition.** Für diese optionale Eigenschaft ist im Skript oben **CenterScreen** festgelegt. Wenn Sie diese Eigenschaft nicht hinzufügen, wählt Windows eine Stelle aus, wenn das Formular geöffnet wird. Durch Festlegen der **StartingPosition** auf **CenterScreen** wird das Formular automatisch bei jedem Laden in der Mitte des Bildschirms angezeigt.

```
$form.Text = "Select a Date" 
$form.Size = New-Object Drawing.Size @(243,230) 
$form.StartPosition = "CenterScreen"
```

Als Nächstes erstellen Sie in Ihrem Formular ein Kalendersteuerelement. In diesem Beispiel wird der aktuelle Tag nicht hervorgehoben oder markiert. Benutzer können immer nur einen Tag im Kalender auswählen.

```
$calendar = New-Object System.Windows.Forms.MonthCalendar 
$calendar.ShowTodayCircle = $False
$calendar.MaxSelectionCount = 1
$form.Controls.Add($calendar)
```

Als Nächstes erstellen Sie eine Schaltfläche **OK** für Ihr Formular. Legen Sie die Größe und das Verhalten der Schaltfläche **OK** fest. In diesem Beispiel wird als Position der Schaltfläche 165 Pixel vom oberen Rand des Formulars und 38 Pixel vom linken Rand entfernt festgelegt. Die Schaltflächenhöhe beträgt 23 Pixel und die Schaltflächenlänge 75 Pixel. Das Skript verwendet vordefinierte Windows-Formulartypen zur Bestimmung des Schaltflächenverhaltens.

```
$OKButton = New-Object System.Windows.Forms.Button
$OKButton.Location = New-Object System.Drawing.Point(38,165)
$OKButton.Size = New-Object System.Drawing.Size(75,23)
$OKButton.Text = "OK"
$OKButton.DialogResult = [System.Windows.Forms.DialogResult]::OK
$form.AcceptButton = $OKButton
$form.Controls.Add($OKButton)
```

In entsprechender Weise erstellen Sie eine Schaltfläche **Abbrechen**. Die Position der Schaltfläche **Abbrechen** ist 165 Pixel vom oberen Rand und 113 Pixel vom linken Rand des Fensters entfernt.

```
$CancelButton = New-Object System.Windows.Forms.Button
$CancelButton.Location = New-Object System.Drawing.Point(113,165)
$CancelButton.Size = New-Object System.Drawing.Size(75,23)
$CancelButton.Text = "Cancel"
$CancelButton.DialogResult = [System.Windows.Forms.DialogResult]::Cancel
$form.CancelButton = $CancelButton
$form.Controls.Add($CancelButton)
```

Legen Sie die Eigenschaft **Topmost** auf **$True** fest, um zu erzwingen, dass das Fenster über anderen geöffneten Fenstern und Dialogfeldern geöffnet wird.

```
$form.Topmost = $True
```

Fügen Sie die folgende Codezeile hinzu, um das Formular in Windows anzuzeigen.

```
$result = $form.ShowDialog()
```

Abschließend weist der Code im Block **If** Windows an, was mit dem Formular geschehen soll, wenn Benutzer einen Tag im Kalender auswählen und anschließend auf die Schaltfläche **OK** klicken oder die **EINGABETASTE** drücken. Windows PowerShell zeigt den Benutzern das ausgewählte Datum an.

```
if ($result -eq [System.Windows.Forms.DialogResult]::OK)
{
    $date = $calendar.SelectionStart
    Write-Host "Date selected: $($date.ToShortDateString())"
}
```

## Weitere Informationen
[Hey Scripting Guy: Warum funktionieren diese PowerShell GUI-Beispiele nicht?](http://go.microsoft.com/fwlink/?LinkId=506644)
[GitHub: WinFormsExampleUpdates von Dave Wyatt](https://github.com/dlwyatt/WinFormsExampleUpdates)
[Windows PowerShell-Tipp der Woche: Erstellen einer grafischen Datumsauswahl](http://technet.microsoft.com/library/ff730942.aspx)




<!--HONumber=Oct16_HO1-->


