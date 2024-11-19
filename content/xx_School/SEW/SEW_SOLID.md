#Software-Engineering 

---
## # Softwarequalität

**SOLID** = steht für solide Grundprinzipe, an die man sich beim Schreiben von Code halten sollte

1. Single Responsibility Prinzip
2. Open Closed Prinzip
3. L. Substitutions

---
### # Single Responsibility Prinzip

Jedes Softwareelement implementiert nur einen Aspekt. Siehe [[SEW_Koppelung-Kohäsion]]
- erhöht Wartbarkeit

Bsp.: Eine Klasse kümmert sich nur um eine Aufgabe - nicht um mehr!

---
### # Interface Segregation Prinzip

[CQRS](https://entwickler.de/software-architektur/gemeinsam-mehr-erreichen-001)

Interface - so "schlank" wie nötig. Eine bestimmte Teilaufgabe soll abgedeckt werden.
Weil eine Klasse könnte mehrere Interfaces implementieren. 

Bsp.: Datenzugriffsschicht - CRUD-Methoden (wir geben CRUD-Methoden in Interface)
- beide Methoden können gleiches Interface implementieren: IDatenzugriff
- IDatenzugriff unterteilen - IReadDatenzugriff, IWriteDatenzugriff
- je nachdem was Verwender benötigt muss daher nicht alles implementiert werden

Verstärkung von **Kohäsion**
Schwächen von **Koppelung**

---
### # Open Closed Prinzip

steht eigentlich für "**Open** for **Extension** but **Closed** for **Modification**"

Im Code-Bsp. in Skriptum>rechts: Es muss nur um eine Methode erweitert werden. 

```csharp
...
// Erweiterung
public class TypeFilter : Predicate<Apple> {...}
...
// Bestehender Code muss nicht erweitert werden
```

```csharp
// Verletzung v. Open Closed Prinzip
class Shape { }

class Circle : Shape
{
	public double Radius {get;set;}
}

class Square : Shape
{
	public double SideLength {get;set}
}

class AreaCalculator
{
	public double CalculateArea(Shape s)
	{
		if(s is Circle c)
		{
			return c.Radius * c.Radius * Math.PI;
		}
		else if(s is Square sq)
		{
			return sq.SideLength * sq.SideLength;
		}
		else
		{
			throw new Exception("Shape unknown");
		}
	}
}
```

```csharp
abstract class Shape 
{ 
	public abstract double CalculateArea(); 
} 

class Circle : Shape 
{ 
	public double Radius { get; set; } 
	public override double CalculateArea() 
	{ 
		return Radius * Radius * Math.PI; 
	} 
} 

class Square : Shape 
{ 
	public double SideLength { get; set; } 
	public override double CalculateArea() 
	{ 
		return SideLength * SideLength; 
	} 
} 

class AreaCalculator 
{ 
	public double CalculateArea(Shape shape)
	{ 
		return shape.CalculateArea(); 
	} 
}
```

