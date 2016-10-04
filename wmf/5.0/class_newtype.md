# Neue Sprachfeatures in PowerShell 5.0 

PowerShell 5.0 führt die folgenden neuen Sprachelemente in Windows PowerShell ein:

## Schlüsselwort „Class“

Das Schlüsselwort **class** definiert eine neue Klasse. Es handelt sich um einen echten .NET Framework-Typ. Klassenmember sind öffentlich, jedoch nur innerhalb des Geltungsbereichs des Moduls.
Sie können auf den Typnamen nicht mittels einer Zeichenfolge verweisen (`New-Object` funktioniert z. B. nicht). In dieser Version können Sie zudem keinen Literaltyp (z. B. `[MyClass]`) außerhalb der Skript-/Moduldatei verwenden, in der die Klasse definiert ist.

```powershell
class MyClass
{
    ...
}
```

## Schlüsselwort „enum“ und Enumerationen

Das Schlüsselwort **enum** wurde hinzugefügt, welches das Zeilenumbruchzeichen als Trennzeichen verwendet.
Aktuelle Einschränkungen: Sie können einen Enumerator nicht hinsichtlich sich selbst definieren, aber Sie können, wie im folgenden Beispiel gezeigt, eine Enumeration hinsichtlich einer anderen Enumeration initialisieren.
Darüber hinaus kann der Basistyp derzeit nicht angegeben werden. Er ist stets [int].

```powershell
enum Color2
{
    Yellow = [Color]::Blue
}
```

Ein Enumeratorwert muss eine Konstante zur Analysezeit sein. Sie können ihn nicht auf das Ergebnis eines aufgerufenen Befehls festlegen.

```powershell
enum MyEnum
{
    Enum1
    Enum2
    Enum3 = 42
    Enum4 = [int]::MaxValue
}
```

Enumerationen unterstützen arithmetische Operationen, wie im folgenden Beispiel dargestellt.

```powershell
enum SomeEnum { Max = 42 }
enum OtherEnum { Max = [SomeEnum]::Max + 1 }
```

## Import-DscResource

**Import-DscResource** ist jetzt ein tatsächlich dynamisches Schlüsselwort.
PowerShell analysiert das Stammmodul des angegebenen Moduls und sucht Klassen, die das **DscResource**-Attribut enthalten.

## ImplementingAssembly

Das neue Feld **ImplementingAssembly** wurde „ModuleInfo“ hinzugefügt. Es ist auf die dynamische Assembly, die für ein Skriptmodul erstellt wird, wenn das Skript Klassen definiert, oder die geladene Assembly für binäre Module festgelegt. Falls „ModuleType = Manifest“, wird es nicht festgelegt. 

Über eine Reflektion auf das Feld **ImplementingAssembly** werden Ressourcen in einem Modul ermittelt. Dies bedeutet, dass Sie Ressourcen ermitteln können, die in PowerShell oder anderen verwalteten Sprachen geschrieben wurden.

Felder mit Initialisierern:      

```powershell
[int] $i = 5
```

„Static“ wird unterstützt und funktioniert wie Typeinschränkungen wie ein Attribut, weshalb die Angabe in beliebiger Reihenfolge erfolgen kann.

```powershell
static [int] $count = 0
```

Ein Typ ist optional.

```powershell
$s = "hello"
```

Alle Member sind öffentlich. 

## Konstruktoren und Instanziierung

Windows PowerShell-Klassen können Konstruktoren haben, die den gleichen Namen wie ihre Klasse haben. Konstruktoren können überladen werden. Statische Konstruktoren werden unterstützt. Eigenschaften mit Initialisierungsausdrücken werden vor dem Ausführen von Code in einem Konstruktor initialisiert. Statische Eigenschaften werden vor dem Hauptteil eines statischen Konstruktors initialisiert. Instanzeigenschaften werden vor dem Hauptteil des nicht statischen Konstruktors initialisiert. Derzeit gibt es keine Syntax zum Aufrufen eines Konstruktors aus einem anderen Konstruktor (wie die C\#-Syntax „: this()“). Eine Behelfslösung ist das Definieren einer allgemeinen „Init“-Methode. 

Es folgen Methoden zum Instanziieren von Klassen in dieser Version.

Instanziieren mithilfe des Standardkonstruktors. Beachten Sie, dass „New-Object“ in dieser Version nicht unterstützt wird.

```powershell
$a = [MyClass]::new()
```

Aufruf eines Konstruktors mit einem Parameter

```powershell
$b = [MyClass]::new(42)
```

Übergeben ein Arrays an einen Konstruktor mit mehreren Parametern
```powershell
$c = [MyClass]::new(@(42,43,44), "Hello")
```

In dieser Version funktioniert „New-Object“ nicht mit Klassen, die in Windows PowerShell definiert werden. Außerdem ist bei dieser Version der Typname nur lexikalisch sichtbar, was bedeutet, dass er außerhalb des Moduls oder Skripts, das die Klasse definiert, nicht sichtbar ist. Funktionen können Instanzen einer Klasse zurückgeben, die in Windows PowerShell definiert wurden. Instanzen funktionieren auch außerhalb des Moduls oder Skripts.

`Get-Member -Static` listet Konstruktoren auf, sodass Sie Überladungen wie andere Methoden anzeigen können. Die Leistung dieser Syntax ist auch erheblich schneller als „New-Object“.

Die pseudostatische Methode **new** funktioniert mit .NET-Typen, wie im folgenden Beispiel gezeigt.

```powershell
[hashtable]::new()
```

Sie können jetzt Konstruktorüberladungen mit „Get-Member“ oder wie in diesem Beispiel anzeigen:

```powershell
PS> [hashtable]::new
OverloadDefinitions
-------------------
hashtable new()
hashtable new(int capacity)
hashtable new(int capacity, float loadFactor)
```

## Methoden

Eine Windows PowerShell-Klassenmethode wird als Skriptblock mit nur einem „end“-Block implementiert. Alle Methoden sind öffentlich. Nachstehend sehen Sie ein Beispiel der Definition einer Methode namens **DoSomething**.

```powershell
class MyClass
{
    DoSomething($x)
    {
        $this._doSomething($x) # method syntax
    }
    private _doSomething($a) {}
}
```

Methodenaufruf:

```powershell
$b = [MyClass]::new()
$b.DoSomething(42) 
```

Überladene Methoden, d. h. Methoden mit demselben Namen wie eine vorhandene Methode, aber mit unterschiedlichen angegebenen Werten, werden ebenfalls unterstützt.

## Eigenschaften 

Alle Eigenschaften sind öffentlich. Eigenschaften erfordern ein Zeilenumbruchzeichen oder Semikolon. Wenn kein Objekttyp angegeben ist, ist der Eigenschaftentyp „object“.

Eigenschaften, die Validierungs- oder Argumenttransformationsattribute verwenden (z. B. `[ValidateSet("aaa")]`), funktionieren wie erwartet.

## Hidden

Mit **Hidden** wurde ein neues Schlüsselwort hinzugefügt. **Hidden** kann auf Eigenschaften und Methoden (einschließlich Konstruktoren) angewendet werden.

Ausgeblendete Member sind öffentlich, werden aber in der Ausgabe von „Get-Member“ nicht angezeigt, es sei denn, der „-Force“-Parameter wird hinzugefügt.

Ausgeblendete Member sind bei Verwenden der Befehlszeilenergänzung oder von IntelliSense nicht enthalten, es sei denn, die Ergänzung erfolgt in der Klasse, die das ausgeblendete Member definiert.

Das neue Attribut **System.Management.Automation.HiddenAttribute** wurde hinzugefügt, damit C#-Code in Windows PowerShell die gleiche Semantik haben kann.

## Rückgabetypen

Der Rückgabetyp ist ein Vertrag. Der Rückgabewert wird in den erwarteten Typ konvertiert. Falls kein Rückgabetyp angegeben wird, ist der Rückgabetyp „void“. Es gibt kein Streaming von Objekten. Objekte können nicht absichtlich oder versehentlich in die Pipeline geschrieben werden.

## Attributes

Die beiden neuen Attribute **DscResource** und **DscProperty** wurden hinzugefügt.

## Lexikalische Eingrenzung von Variablen

Das folgende Beispiel zeigt, wie die lexikalische Eingrenzung in dieser Version funktioniert.

```powershell
$d = 42 # Script scope

function bar
{
    $d = 0 # Function scope
    [MyClass]::DoSomething()
}

class MyClass
{
    static [object] DoSomething()
    {
        return $d # error, not found dynamically
        return $script:d # no error
        $d = $script:d
        return $d # no error, found lexically
    }
}

$v = bar
$v -eq $d # true
```

## End-to-End-Beispiel

Das folgende Beispiel erstellt mehrere neue, benutzerdefinierte Klassen, um eine HTML-Sprache des Typs DSL (Dynamic Style Sheet Language) zu implementieren. Dann werden im Beispiel Hilfsfunktionen hinzugefügt, um spezifische Elementtypen als Teil der Elementklasse zu erstellen, wie z. B. Überschriftsformate und Tabellen, damit Typen nicht außerhalb des Geltungsbereichs eines Moduls verwendet werden können.

```powershell
# Classes that define the structure of the document
#
class Html
{
    [string] $docType
    [HtmlHead] $Head
    [Element[]] $Body
    
    [string] Render()
    {
        $text = "<html>`n<head>`n"
        $text += $this.Head
        $text += "`n</head>`n<body>`n"
        $text += $this.Body -join "`n" # Render all of the body elements
        $text += "</body>`n</html>"
        return $text
    }
    [string] ToString() { return $this.Render() }
}

class HtmlHead
{
    $Title
    $Base
    $Link
    $Style
    $Meta
    $Script
    [string] Render() { return "<title>$($this.Title)</title>" }
    [string] ToString() { return $this.Render() }
}

class Element
{
    [string] $Tag
    [string] $Text
    [hashtable] $Attributes
    [string] Render() {
        $attributesText= ""
        if ($this.Attributes)
        {
            foreach ($attr in $this.Attributes.Keys)
            {
                #BUGBUG - need to validate keys against attribute
                $attributesText += " $attr=`"$($this.Attributes[$attr])\`""
            }
        }
        return "<$($this.tag)${attributesText}>$($this.text)</$($this.tag)>`n"
    }
[string] ToString() { return $this.Render() }
}

#
# Helper functions for creating specific element types on top of the classes.
# These are required because types aren’t visible outside of the module.
#

function H1 { [Element] @{ Tag = "H1" ; Text = $args.foreach{$_} -join " " }}
function H2 { [Element] @{ Tag = "H2" ; Text = $args.foreach{$_} -join " " }}
function H3 { [Element] @{ Tag = "H3" ; Text = $args.foreach{$_} -join " " }}
function P { [Element] @{ Tag = "P" ; Text = $args.foreach{$_} -join " " }}
function B { [Element] @{ Tag = "B" ; Text = $args.foreach{$_} -join " " }}
function I { [Element] @{ Tag = "I" ; Text = $args.foreach{$_} -join " " }}
function HREF
{
    param (
        $Name,
        $Link
    )
    return [Element] @{
        Tag = "A"
        Attributes = @{ HREF = $link }
        Text = $name
    }
}
function Table
{
    param (
    [Parameter(Mandatory)]
    [object[]]
        $Data,
    [Parameter()]
    [string[]]
        $Properties = "*",
    [Parameter()]
    [hashtable]
        $Attributes = @{ border=2; cellpadding=2; cellspacing=2 }
    )
$bodyText = ""
# Add the header tags
$bodyText += $Properties.foreach{TH $_}
# Add the rows
$bodyText += foreach ($row in $Data)
    {
        TR (-join $Properties.Foreach{ TD ($row.$\_) } )
    }

    $table = [Element] @{
        Tag = "Table"
        Attributes = $Attributes
        Text = $bodyText
    }
$table
}
function TH { ([Element] @{ Tag = "TH" ; Text = $args.foreach{$_} -join " " }) }
function TR { ([Element] @{ Tag = "TR" ; Text = $args.foreach{$_} -join " " }) }
function TD { ([Element] @{ Tag = "TD" ; Text = $args.foreach{$_} -join " " }) }
function Style

{
    return [Element] @{
        Tag = "style"
        Text = "$args"
    }
}

# Takes a hash table, casts it to and HTML document
# and then returns the resulting type.
#
function Html ([HTML] $doc) { return $doc }
```

<!--HONumber=Oct16_HO1-->


