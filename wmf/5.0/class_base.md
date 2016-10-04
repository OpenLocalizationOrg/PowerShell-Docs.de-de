# Deklarieren der Basisklasse
Sie können eine Windows PowerShell-Klasse als Basistyp für eine andere Windows PowerShell-Klasse deklarieren.

```PowerShell
class bar
{
   [int]foo() 
       {
           return 100500
       }
}

class baz : bar {}

[baz]::new().foo() # return 100500
```

Sie können auch vorhandene .NET Framework-Typen als Basisklassen verwenden:

```PowerShell
class MyIntList : system.collections.generic.list[int]
{
    
}

$list = [MyIntList]::new()

$list.Add(100)

$list[0] # return 100
```

<!--HONumber=Oct16_HO1-->


