#school 

---
## # Metriken

Macht Software anhand von Maßzahl messbar bzw. vergleichbar.
Wir schauen auf Qualitäts-Metriken v. Programmcode
- Koppelung
- Kohäsion

---
## # Koppelung

Maß der Abhängigkeit zwischen Softwareelementen. 

>Anstreben von: Loose Coupling

Es gibt 2 Stufen von Koppelung:

1. Tight Coupling
	- "starke Koppelung"
	  tritt auf, wenn eine `Klasse direkt auf Implementierungen anderer Klassen zugreift` = _Wartbarkeit wird beeinträchtigt_
2. Loose Coupling
	- "schwache Koppelung"
	  Klassen haben nur minimale Kenntnisse zu anderen Klassen
	  Zugriff auf `Funktionalität läuft über Interfaces` = _flexibel & wartungsfreundlich_

2 unterschiedliche Arten v. Koppelung:
1. Interaktionskoppelung
	- Maß an Funktionalität eines anderen Objekt, Klasse
	- tritt auf, wenn Objekte einer Klasse, Methoden von Objekten anderer Klassen aufrufen
2. Vererbungskoppelung
	- Ausmaß der Abhängigkeit der erbenden & der Basisklasse (falsche Vererbung)
	- tritt auf, wenn ein Objekt einer Klasse direkt auf ein anderes Objekt einer Klasse verweist (erbt)

---
### # Interaktionskoppelung

Passiert bei direkter Abhängigkeit von einer Klasse auf eine andere Klasse.
Das macht beide Klassen voneinander abhängig.

Einfache Beispiele:

```csharp
// Direkte Abhängigkeit 
// Änderungen in B können dazu führen, dass auch A geändert werden muss
public class A { private B b = new B(); }  

// Direkte Nutzung
// Erneut: A ist davon abhängig, wie B implementiert ist
public class A 
{
	public void DoSomething(B b) => b.SomeMethod();
}
```

```csharp
public class Portfolio  
{  
    private string Name { get; set; }  
    public decimal LastNotifiedPrice { get; private set; }  
  
    public Portfolio(string name)  
    {  
        Name = name;  
    }  
  
    public void NotifyPriceChange(decimal newPrice)  
    {  
        LastNotifiedPrice = newPrice;  
        Console.WriteLine($"Portfolio '{Name}' wurde über den neuen Preis {newPrice:C} informiert.");  
    }  
}  
  
public class StockMarket  
{  
    private decimal _price;  
  
    protected List<Portfolio> Portfolios { get; set; } = new();  
  
    public decimal Price  
    {  
        set  
        {  
            _price = value;  
  
            // Interaktionskoppelung:  
            // Wenn es notwendig wird, auf Änderungen            
            // im Kurs anders zu reagieren, muss die
            // Codebasis der Klasse geändert werden.
            Portfolios.ForEach(p => p.NotifyPriceChange(value));  
        }  
    }}
```

### # Beispiel ohne Interaktionskoppelung

Umgehen von Interaktionskoppelung durch Observer-Pattern (siehe Skizze).

![[SEW_Softwaremetriken 2024-11-20 19.13.15.excalidraw]]

```csharp
namespace KoppelungClasses;  
  

public interface IObserver  
{  
	void Update(decimal value);  
} 

public class Portfolio(string name) : IObserver  
{  
	private string Name { get; set; } = name;  
	public decimal LastNotifiedPrice { get; private set; }  

	public void Update(decimal value)  
	{  
		LastNotifiedPrice = value;  
		Console.WriteLine($"Portfolio '{Name}' wurde über den neuen Preis {value} informiert.");  
	}  
}  

public class Bank(string name) : IObserver  
{  
	private string Name { get; set; } = name;  

	public void Update(decimal value)  
	{  
		Console.WriteLine($"Bank {Name} wurde über Preis {value} benachrichtigt!");  
	}  
}    

public class StockMarket  
{  
	private decimal price;  
	private readonly List<IObserver> observers = [];  

	public void Subscribe(IObserver observer) => observers.Add(observer);  
	public void Unsubscribe(IObserver observer) => observers.Remove(observer);  
	  
	private void NotifyObservers(decimal value)  
	{  
		foreach (var observer in observers)  
		{  
			observer.Update(value);  
		}  
	}        public decimal Price  
	{  
		set  
		{  
			price = value;  
			NotifyObservers(value);  
		}  
	}   
}
```

Ein bisschen umgewandelt (laut sew4-Skriptum):

```csharp
namespace KoppelungClasses;  
  
public class OhneInteraktionsKoppelungV2  
{  
    public interface IObserver  
    {  
        void Update(decimal price);  
    }  
  
    private interface IObservable  
    {  
        void AddObserver(IObserver observer);  
        void RemoveObserver(IObserver observer);  
        void NotifyObserver();  
    }  
  
    public class Portfolio(string name) : IObserver  
    {  
        private string Name { get; set; } = name;  
  
        public void Update(decimal price) => Console.WriteLine($"{Name} notified about new price: {price}");  
    }  
  
    public class Bank(string name) : IObserver  
    {  
        private string Name { get; set; } = name;  
  
        public void Update(decimal price) => Console.WriteLine($"{Name} notified about new price: {price}");  
    }  
  
    public class StockMarket : IObservable  
    {  
        private decimal price;  
        private List<IObserver> observers = [];  
  
        public void AddObserver(IObserver observer) => observers.Add(observer);  
        public void RemoveObserver(IObserver observer) => observers.Remove(observer);  
        public void NotifyObserver() => observers.ForEach(o => o.Update(price));  
          
        public decimal Price  
        {  
            get => price;  
            set  
            {  
                price = value;  
                NotifyObserver();  
            }  
        }    
    }
}
```

---
### # Vererbungskoppelung

Wenn von einer Basisklasse die geerbten Methoden in den Kindklassen zur Gänze überschrieben werden verliert Vererbung ihren Sinn = Vererbungskoppelung tritt auf.
Mithilfe von Objektkomposition kann man diese auflösen.

Beispiel:

- Hier ist Fly() unpassend für die RubberDuck beispielsweise. 
  Es entsteht also eine unnötige Abhängigkeit, weil RubberDuck dieses Verhalten nicht braucht/umsetzen kann.

```csharp
public class Vererbungskoppelung  
{  
    public class Duck  
    {  
        public string Quack() => "quack";  
        public String Fly() => "fly high in the sky";  
    }  
    public class ReadHeadDuck : Duck  
    {  
        public string Quack() => "loudly quack";  
    }  
    public class RubberDuck : Duck  
    {  
        public String Quack() => "squeeze";  
        public String Fly() => "can't fly";  
    }  
}
```

Guter Lösungsansatz - Objektkomposition.
Einfach erweiterbar & wartungsfreundlich. Der bestehende Code muss bei Erweiterung nicht geändert werden.
Verwendung von Objektkomposition/"Strategie-Pattern" - Verhalten von Objekt kann zur Laufzeit verändert werden. Duck Klasse weist "Strategien" (Quack, Fly) per interfaces dynamisch zu.

![[SEW_Softwaremetriken 2024-11-20 21.04.38.excalidraw]]

```csharp
namespace KoppelungClasses;  
  
public class OhneVererbungskoppelung  
{  
    public interface IQuackable  
    {  
        string Quack();  
    }  
    public interface IFlyable  
    {  
        string Fly();  
    }  
    
    public class DefaultQuackBehaviour : IQuackable  
    {  
        public string Quack() => "quack";  
    }  
    public class LoudQuackBehaviour : IQuackable  
    {  
        public string Quack() => "loudly: quack";  
    }  
    public class ProudQuackBehaviour : IQuackable  
    {  
        public string Quack() => "proudly: quack";  
    }  
    
    public class DefaultFlyBehaviour : IFlyable  
    {  
        public string Fly() => "flying high in the sky";  
    }  
    public class NoFlyBehaviour : IFlyable  
    {  
        public string Fly() => "can't fly";  
    }  
    
    public class Duck  
    {  
        public IQuackable QuackBehaviour { get; set; }  
        public IFlyable FlyBehaviour { get; set; }  
    }    
    public class DuckFactory  
    {  
        public static Duck CreateRedheadDuck() => new Duck  
        {  
            QuackBehaviour = new LoudQuackBehaviour(),  
            FlyBehaviour = new DefaultFlyBehaviour()  
        };  
        public Duck CreateRubberDuck() => new Duck  
        {  
            QuackBehaviour = new LoudQuackBehaviour(),  
            FlyBehaviour = new NoFlyBehaviour()  
        };  
    }  
}
```

---
## # Kohäsion

Maß des inneren Zusammenhalts eines Softwareelements.

>Anstreben von: Hoher Kohäsion

Definition v. Kohäsion: Klasse sollte nur Methoden/Attribute enthalten, die alle zur Lösung einer gemeinsamen Aufgabe/Verantwortungsbereich gehören.

2 unterschiedliche Arten v. Kohäsion:
1. ServiceKohäsion
	- Beschreibung zu `inneren Zusammenhalt einer Methode`
	- Methoden einer Klasse - nur EINE Lösung von Aufgabe/Problem 
2. Klassenkohäsion
	- Beschreibung zu `inneren Zusammenhalt einer Klasse`
	- Verletzung v. Klassenkohäsion - ungenutzte Attribute/Methoden in Klasse

---
### # Servicekohäsion

```csharp
//------------------------------------------
// Servicekohaesion - schwache Kohäsion
//------------------------------------------
class Vector implements Serializable
{
	public int X { get; set; }
	public int Y { get; set; }

	public float Add(Vector v)
	{
		this.X += v.X;
		this.Y += v.Y;
	
		return Math.SQRT(X * X + Y * Y);
	}
}
```

---