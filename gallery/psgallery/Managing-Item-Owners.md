# Verwalten von Elementbesitzern

Der Besitz eines Elements im PowerShell-Katalog wird von der Person definiert, die das Element im Katalog veröffentlicht hat.
Mitunter müssen diese Metadaten über die anfängliche Veröffentlichung des Elements hinaus verwaltet werden, was bedeutet, dass die Besitzermetadaten veränderlich sein müssen, das Element hingegen nicht.

Alle Elementbesitzer sind Peers. Das bedeutet, dass alle Elementbesitzer eine neue Version eines Elements veröffentlichen können. Es bedeutet aber auch, dass jeder Elementbesitzer jeden anderen Elementbesitzer entfernen kann. Kein Besitzer verfügt über mehr Berechtigungen als andere Besitzer.  

## Festlegen des anfänglichen Besitzers eines Elements 

Beim Veröffentlichen eines neues Elements im PowerShell-Katalog wird der anfängliche Besitzer von dem Benutzer definiert, der das Element veröffentlicht. Dies richtet sich danach, wessen API-Schlüssel im Cmdlet „Publish-Modul“ verwendet wurde.

## Hinzufügen von Besitzern

Sobald ein Element im PowerShell-Katalog veröffentlicht wurde, können ganz einfach zusätzliche Benutzer als Besitzer eines Elements eingeladen werden.

1. [Melden Sie sich beim PowerShell-Katalog mit dem Konto an](https://powershellgallery.com/users/account/LogOn), das der aktuelle Besitzer eines Elements ist.
2. Navigieren Sie zu einer Elementseite, indem Sie die Registerkarte „Elemente“ verwenden, suchen oder auf Ihren Benutzernamen und dann auf [**Meine Elemente verwalten**](https://www.powershellgallery.com/account/Packages) klicken.
3. Wenn Sie als Besitzer eines Elements angemeldet sind, finden Sie auf der linken Seite einen Link „Besitzer verwalten“, auf den Sie klicken können.
4. Geben Sie den Benutzernamen der Person ein, die Sie als Besitzer hinzufügen möchten, und klicken Sie auf „Hinzufügen“.
5. Eine E-Mail wird an den neuen Mitbesitzer gesendet, um ihn als Besitzer eines Elements einzuladen.
6. Sobald der Benutzer auf den Link klickt, ist er ein vollständiger Mitbesitzer mit Vollzugriff auf ein Element, einschließlich der Möglichkeit, andere Benutzer als Besitzer zu entfernen.

**HINWEIS**: Der neue Besitzer wird *erst dann* als Besitzer eines Elements aufgeführt, wenn er den Besitz bestätigt.
Beim Anzeigen der Seite **Besitzer verwalten** sehen Sie bei den aktuellen Besitzern einen Eintrag „Genehmigung steht aus“.
Diese Einladung kann ebenso entfernt werden wie andere Besitzer entfernt werden können.
Dieser Einladungsprozess verhindert, dass Benutzer fälschlicherweise andere Benutzer als Besitzer ihrer Elemente hinzufügen.

Beachten Sie, dass die Metadaten „Autoren“ in reinem Freiformtext vorliegen. Nur „Besitzer“ werden gesteuert.


## Entfernen von Besitzern
Wenn ein Element mehrere Besitzer aufweist und einer entfernt werden muss, ist der Prozess einfach:

1. [Melden Sie sich beim PowerShell-Katalog mit dem Konto an](https://powershellgallery.com/users/account/LogOn), das der aktuelle Besitzer eines Elements ist.
2. Navigieren Sie zu einer Elementseite, indem Sie die Registerkarte „Elemente“ verwenden, suchen oder auf Ihren Benutzernamen und dann auf [**Meine Elemente verwalten**](https://www.powershellgallery.com/account/Packages) klicken.
3. Wenn Sie als Besitzer eines Elements angemeldet sind, finden Sie auf der linken Seite einen Link „Besitzer verwalten“, auf den Sie klicken können.
4. Klicken Sie neben dem Besitzer, der entfernt werden soll, auf den Link „Entfernen“.



## Übertragen des Besitzes für ein Element
Gelegentlich erhalten wir Supportanfragen, um den Besitz eines Elements von einem Benutzer auf einen anderen zu übertragen, allerdings können Sie das fast immer selbst durchführen.
Das Übertragen des Besitzes von einem Benutzer auf einen anderen ist einfach eine Kombination aus den beiden oben genannten Funktionen.

1. Der aktuelle Besitzer lädt den neuen Benutzer als Mitbesitzer ein, und der neue Benutzer akzeptiert die Einladung.
2. Der neue Benutzer entfernt den alten Benutzer aus der Liste der Besitzer.

Diese Anfrage ist über mehrere Formulare eingegangen, der Prozess funktioniert aber immer gleich.

* Der Besitz eines Elements ändert sich von einem Entwickler zu einem anderen.
* Das Element wurde versehentlich über das falsche Konto veröffentlicht.


## Verwaiste Elemente
Ein letztes Szenario ist aufgetreten, wenn auch nicht häufig.
Elemente waren verwaist und das einzige Elementbesitzerkonto kann nicht zum Hinzufügen neuer Besitzer verwendet werden.
Im Folgenden sind einige Beispiele für dieses Szenario aufgeführt:

* Dem Konto des Besitzers ist eine E-Mail-Adresse zugeordnet, die nicht mehr existiert, und der Benutzer hat das Kennwort vergessen.
* Der registrierte Besitzer hat das Unternehmen verlassen, in dem das Element erzeugt wird, und kann nicht erreicht werden, um den Besitz des Elements zu aktualisieren.
* Aufgrund eines Fehlers, der nur wenige Elemente betrifft, befindet sich das Element ohne Besitzer im Katalog.

Die PowerShell-Katalogadministratoren können für jedes Element auf den Link „Besitzer verwalten“ zugreifen.
Wenn Sie der rechtmäßige Besitzer eines Elements sind und den aktuellen Besitzer des Elements nicht erreichen können, um Besitzberechtigungen zu erlangen, wenden Sie sich über den Link „Missbrauch melden“ im Katalog an die PowerShell-Katalogadministratoren.
Wir befolgen dann einen Prozess, um Ihren Besitz des Elements zu überprüfen.
Wenn wir feststellen, dass Sie der Besitzer des Elements sein sollten, verwenden wir selbst den Link „Besitzer verwalten“ für das Element und senden Ihnen die Einladung als Besitzer.
Dies erfolgt jedoch erst, nachdem wir Sie als Besitzer bestätigt haben, und der Vorgang variiert je nach den Umständen.
Oft verwenden wir die Projekt-URL des Elements, um eine Möglichkeit zum Kontaktieren des Projektbesitzers zu finden. Gelegentlich wenden wir uns jedoch auch über Twitter, E-Mail oder andere Methoden an den Besitzer.

<!--HONumber=Oct16_HO1-->


