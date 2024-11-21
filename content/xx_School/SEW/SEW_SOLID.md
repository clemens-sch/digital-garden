#school 

---
## # Softwarequalität

**SOLID** = steht für solide Grundprinzipe, an die man sich beim Schreiben von Code halten sollte

1. Single Responsibility Prinzip
2. Open Closed Prinzip
3. L. Substitutions
4. Interface Segregation Prinzip
5. Dependency Inversion Prinzip

---
### # Single Responsibility Prinzip

Jedes Softwareelement implementiert nur einen Aspekt. Siehe [[SEW_Softwaremetriken]]
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

Steht eigentlich für "**Open** for **Extension** but **Closed** for **Modification**"
Ist mein Code erweiterbar, ohne dass das vorher definierte geändert wird.

Prinzip kann aufgelöst werden mit: Interface, Basisklasse

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

---
### # Liskovsche Substitutionsprinzip

Alles was in Basisklasse definiert/da ist, sollte in den abgeleiteten Klassen auch da sein.
Für Verwender einer Klasse muss egal sein, ob Basisklasse oder abgeleitete Klasse.
Wird oft bei falschen Vererbungshierarchien verletzt.

Bsp.: Datenzugriffsschicht - eine Klasse mit Speichern
- abgeleitete Klasse: speichert in rel. DB
- abgeleitete Klasse: speichert in Dateisystem
- Verwender von abgeleiteter Klasse muss es egal sein, ob es eine Basisklasse oder abgeleitete Klasse ist. 

---
### # Dependency Inversion Prinzip

Keine direkte Abhängigkeit zw. Data Access & Business Logic 

![[SEW_SOLID 2024-11-19 09.09.20.excalidraw]]

```csharp
// Abstraktion
interface IMessageService
{
    void SendMessage(string message);
}

// Konkrete Implementierungen
class EmailService : IMessageService
{
    public void SendMessage(string message)
    {
        Console.WriteLine($"Email sent: {message}");
    }
}

class SmsService : IMessageService
{
    public void SendMessage(string message)
    {
        Console.WriteLine($"SMS sent: {message}");
    }
}

// Hochrangiges Modul
class Notification
{
    private readonly IMessageService _messageService;

    public Notification(IMessageService messageService) // Abhängig von der Abstraktion
    {
        _messageService = messageService;
    }

    public void Notify(string message)
    {
        _messageService.SendMessage(message);
    }
}
```

