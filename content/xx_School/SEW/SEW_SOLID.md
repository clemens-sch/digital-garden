#school 

[YT-Video](https://www.youtube.com/watch?v=kF7rQmSRlq0)

---
## # Softwarequalität

**SOLID** = steht für solide Grundprinzipien, an die man sich beim Schreiben von Code halten sollte

1. Single Responsibility Prinzip
2. Open Closed Prinzip
3. L. Substitutions
4. Interface Segregation Prinzip
5. Dependency Inversion Prinzip

---
### # Single Responsibility Prinzip

Jedes Softwareelement implementiert nur einen Aspekt. Siehe [[SEW_Softwaremetriken]]
- erhöht Wartbarkeit
- vermeiden v. Merge-Conflict

Eine Klasse kümmert sich nur um eine Aufgabe bzw. alles was die Klasse machen soll sollte von der Aufgabe her sehr nahe beinander liegen.

Dieses Beispiel würde das Single Responsibility Prinzip brechen:

```csharp
public class Student
{
	public int StudentId { get; set; }
	public string Name { get; set; }
	public void SaveToDb() { ... }
	public void SendEmail() { ... }
	public void EnrolToCourse() { ... }
}
```

---
### # Open Closed Prinzip

"**Open** for **Extension** but **Closed** for **Modification**"
Ziel: erweiterbarer Code, ohne dass vorher geschriebenes geändert wird.

Prinzip kann aufgelöst werden durch: Interfaces, Basisklassen.

Im Code-Bsp. in Skriptum>rechts: Es muss nur um eine Methode erweitert werden. 

```csharp
...
// Erweiterung
public class TypeFilter : Predicate<Apple> {...}
...
// Bestehender Code muss nicht verändert werden
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

mit abstrakter Basisklasse:

```csharp
public abstract class Shape  
{  
    public abstract double CalculateArea();  
}  
  
public class Circle(double radius) : Shape  
{  
    private double Radius { get; set; } = radius;  
    public override double CalculateArea() => Radius * Radius * Math.PI;  
}  
  
public class Square(double sideLength) : Shape  
{  
    private double Sidelength { get; set; } = sideLength;  
    public override double CalculateArea() => Sidelength * Sidelength;  
}  
  
public static class AreaCalculator  
{  
    public static double CalculateArea(Shape shape) => shape.CalculateArea();  
}
```

mit Interface:

```csharp
public interface IShape  
{  
    double CalculateArea();  
}  
  
public class Circle(double radius) : IShape  
{  
    private double Radius { get; set; } = radius;  
    public double CalculateArea() => Radius * Radius * Math.PI;  
}  
  
public class Shape(double sideLength) : IShape  
{  
    private double SideLength { get; set; } = sideLength;  
    public double CalculateArea() => SideLength * SideLength;  
}  
  
public static class CalcAreaOfShape  
{  
    public static double CalcArea(IShape shape) => shape.CalculateArea();  
}
```

---
### # Liskovsche Substitutionsprinzip

Alles was in Basisklasse definiert/da ist, sollte in den abgeleiteten Klassen Sinn machen.
Instanzen einer abgeleiteten Klasse müssen sich genauso verhalten wie Objekte der Basisklasse.
Verwender muss es egal sein, ob Basisklasse oder abgeleitete Klasse.

Wird oft bei falschen Vererbungshierarchien verletzt.

Bsp. von Verstoß gg. Liskovsches Substitutions-Prinzip:

```csharp
public class Parent
{
	public void Eat() {...}
	public void Sleep() {...}
	public void GoToWork() {...}
	public void MakeDinner() {...}
}

public class Child : Parent
{...}
```

korrekte Lösung:

```csharp
public class Human
{
	public void Eat() {...}
	public void Sleep() {...}
}

public class Parent : Human
{
	public void GoToWork() {...}
	public void MakeDinner() {...}
}

public class Child : Human
{
	public void PlayWithToys() {...}
}
```

---
### # Interface Segregation Prinzip

[CQRS](https://entwickler.de/software-architektur/gemeinsam-mehr-erreichen-001)

Interface - so "schlank" wie nötig. Eine bestimmte Teilaufgabe soll abgedeckt werden.
> Natürlich das  ganze sollte man auch nicht übertreiben.

Eine Klasse könnte mehrere Interfaces implementieren. 

Bsp.: Datenzugriffsschicht - CRUD-Methoden (wir geben CRUD-Methoden in Interface)
- beide Methoden können gleiches Interface implementieren: IDatenzugriff
- IDatenzugriff unterteilen - IReadDatenzugriff, IWriteDatenzugriff
- je nachdem was Verwender benötigt, muss daher nicht alles implementiert werden

Anstatt einem Interface:

```csharp
public interface IWorker 
{ 
	void Work(); 
	void Manage(); 
	void Report(); 
}
```

Unterteilen in mehrere Interfaces:

```csharp
public interface IWorker 
{ 
	void Work(); 
} 

public interface IManager 
{ 
	void Manage(); 
} 

public interface IReporter 
{ 
	void Report(); 
}
```

---
### # Dependency Inversion Prinzip

"Higher Level components should not depend on lower Level components - both should depend on the interface"
Keine direkte Abhängigkeit zw. Data Access & Business Logic 

![[SEW_SOLID 2024-11-19 09.09.20.excalidraw]]

```csharp
// Abstraktion
interface IMessageService
{
    void SendMessage(string message);
}

// lower order
class EmailService : IMessageService
{
    public void SendMessage(string message)
    {
        Console.WriteLine($"Email sent: {message}");
    }
}

// lower order
class SmsService : IMessageService
{
    public void SendMessage(string message)
    {
        Console.WriteLine($"SMS sent: {message}");
    }
}

// higher order
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
