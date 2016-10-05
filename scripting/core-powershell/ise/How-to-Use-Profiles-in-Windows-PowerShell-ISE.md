---
title: 'So wird''s gemacht: Verwenden von Profilen in Windows PowerShell ISE'
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 0219626a-6da5-4acc-b630-d058e8b29cc6
translationtype: Human Translation
ms.sourcegitcommit: a656ec981dc03fd95c5e70e2d1a2c741ee1adc9b
ms.openlocfilehash: 3543503655cc506c4c0f8a7f3d1056c05a66ecba

---

# So wird's gemacht: Verwenden von Profilen in Windows PowerShell ISE
In diesem Thema wird erklärt, wie Profile in Windows PowerShell® Integrated Scripting Environment (ISE) verwendet werden können. Es empfiehlt sich, dass Sie die Aufgaben in diesem Abschnitt erst ausführen, nachdem Sie [about_Profiles [v4]](https://technet.microsoft.com/library/e1d9e30a-70cc-4f36-949f-fc7cd96b4054(v=wps.630)) gelesen oder im Konsolenbereich `Get-Help about_Profiles` eingegeben und die **EINGABETASTE** gedrückt haben.

Ein Profil ist ein Windows PowerShell ISE-Skript, das automatisch ausgeführt wird, wenn Sie eine neue Sitzung starten.  Sie können ein oder mehrere Windows PowerShell-Profile für Windows PowerShell ISE erstellen und diese dazu verwenden, die Umgebung von Windows PowerShell oder Windows PowerShell ISE zu konfigurieren, um sie mit den Variablen, Aliasen, Funktionen sowie Farb- und Schriftartvoreinstellungen vorzubereiten, die Sie zur Verfügung haben möchten. Ein Profil wirkt sich auf jede Windows PowerShell ISE-Sitzung aus, die Sie starten.

> [!NOTE]
> Die Windows PowerShell-Ausführungsrichtlinie bestimmt, ob Sie Skripts ausführen und ein Profil laden dürfen. Die Standardausführungsrichtlinie, „Restricted“, verhindert das Ausführen jeglicher Skripts, einschließlich Profile. Wenn Sie die Richtlinie „Restricted“ verwenden, kann das Profil nicht geladen werden. Weitere Informationen zu Ausführungsrichtlinien finden Sie unter [about_Execution_Policies [v4]](https://technet.microsoft.com/library/347708dc-1515-4d74-978b-8334603472e6(v=wps.630)).

## Auswählen eines Profils, das in Windows PowerShell ISE verwendet werden soll
Windows PowerShell ISE unterstützt Profile für den aktuellen Benutzer sowie für alle Benutzer. Außerdem werden die Windows PowerShell-Profile unterstützt, die für alle Hosts gelten.

Das Profil, das Sie verwenden, wird durch die Verwendung der Windows PowerShell und Windows PowerShell ISE bestimmt.

-   Wenn Sie nur Windows PowerShell ISE verwenden, um Windows PowerShell auszuführen, speichern Sie alle ihre Elemente in einem ISE-spezifischen Profil, z.B. dem Profil „CurrentUserCurrentHost“ für Windows PowerShell ISE oder „AllUsersCurrentHost“ für Windows PowerShell ISE.

-   Wenn Sie mehrere Hostprogramme verwenden, um Windows PowerShell auszuführen, speichern Sie Ihre Funktionen, Aliase, Variablen und Befehle in einem Profil, das alle Hostprogramme betrifft, z.B. dem Profil „CurrentUserAllHosts“ oder „AllUsersAllHosts“. Speichern Sie ISE-spezifische Merkmale wie Anpassungen von Farbe und Schriftart im Profil „CurrentUserCurrentHost“ für Windows PowerShell ISE oder „AllUsersCurrentHost“ für Windows PowerShell ISE.

Die folgenden Profile sind Profile, die in Windows PowerShell ISE erstellt und verwendet werden können. Jedes Profil wird in seinem eigenen speziellen Pfad gespeichert.

| Profiltyp | Profilpfad |
| --- | --- |
| **Aktueller Benutzer, PowerShell ISE**| `$PROFILE.CurrentUserCurrentHost`, oder `$PROFILE` |
| **Alle Benutzer, die PowerShell ISE**| `$PROFILE.AllUsersCurrentHost` |
| **Aktueller Benutzer, alle hosts**| `$PROFILE.CurrentUserAllHosts` |
| **Alle Benutzer, die alle hosts** | `$PROFILE.AllUsersAllHosts` |

## So erstellen Sie ein neues Profil
Um ein neues „Aktueller Benutzer, PowerShell ISE“-Profil zu erstellen, führen Sie den folgenden Befehl aus:

```PowerShell
if (!(Test-Path -Path $PROFILE )) 
{ New-Item -Type File -Path $PROFILE -Force }
```

Um ein neues „Alle Benutzer, PowerShell ISE“-Profil zu erstellen, führen Sie den folgenden Befehl aus:

```PowerShell
if (!(Test-Path -Path $PROFILE.AllUsersCurrentHost)) 
{ New-Item -Type File -Path $PROFILE.AllUsersCurrentHost -Force }
```

Um ein neues „Aktueller Benutzer, alle Hosts“-Profil zu erstellen, führen Sie den folgenden Befehl aus:

```PowerShell
if (!(Test-Path -Path $PROFILE.CurrentUserAllHosts)) 
{ New-Item -Type File -Path $PROFILE.CurrentUserAllHosts -Force }
```

Um ein neues „Alle Benutzer, alle Hosts“-Profil zu erstellen, geben Sie Folgendes ein:

```PowerShell
if (!(Test-Path -Path $PROFILE.AllUsersAllHosts)) 
{ New-Item -Type File -Path $PROFILE.AllUsersAllHosts -Force }
```

## So bearbeiten Sie ein Profil

1.  Um das Profil zu öffnen, führen Sie den Befehl „psedit“ mit der Variablen aus, die das Profil angibt, das Sie bearbeiten möchten. Wenn Sie beispielsweise das „Aktueller Benutzer, PowerShell ISE“-Profil öffnen möchten, geben Sie Folgendes ein: `psEdit $PROFILE`

2.  Fügen Sie dem Profil einige Elemente hinzu. Es folgen einige Beispiele, die Ihnen den Einstieg erleichtern sollen:

    -   Um die Standardhintergrundfarbe des Konsolenbereichs in Blau zu ändern, geben Sie Folgendes in die Profildatei ein: `$psISE.Options.OutputPaneBackground = 'blue'`. Weitere Informationen zu der Variablen „$psISE“ finden Sie unter [Referenz zum Windows PowerShell ISE-Objektmodell](#windows-powershell-ise-object-model-reference).

    -   Um den Schriftgrad in 20 zu ändern, geben Sie Folgendes in die Profildatei ein: `$psISE.Options.FontSize =20`

3.  Um Ihre Profildatei zu speichern, klicken Sie im Menü **Datei** auf **Speichern**. Wenn Sie Windows PowerShell ISE das nächste Mal öffnen, werden Ihre Anpassungen angewendet.

## Weitere Informationen
- [About_Profiles [v4]](https://technet.microsoft.com/library/e1d9e30a-70cc-4f36-949f-fc7cd96b4054(v=wps.630))
- [Mithilfe von Windows PowerShell ISE](Using-the-Windows-PowerShell-ISE.md)




<!--HONumber=Oct16_HO1-->


