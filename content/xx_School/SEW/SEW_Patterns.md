#school 

---
## # Creational, Structural, Behavioral

1. **Creational Patterns** 
	1. unterstützen Erzeugen komplexer Objekte (Kapselung v. Erzeugungsprozess)
	2. hohe Flexibilität & Wiederverwendbarkeit
2. **Structural Patterns** 
	1. erleichtern Entwurf von Software - Vorgabe v. Bzh. zw. Klassen
	2. Zusammensetzen v. Objekten & Klassen zu größeren Strukturen
3. **Behavioral Patterns** 
	1. modellieren Verhalten unter Softwaresystemen - Interaktionen zw. Objekten
	2. Zuweisung v. Verantwortlichkeit zw. Objekten

Wir haben folgene Patterns durchgemacht:

| Creational | Structural | Behavioral |
| ---------- | ---------- | ---------- |
| Singleton  | Adapter    | Strategy   |
| Factory    | Decorator  | Command    |

---
## # Patterns
### # Singleton

Definiert Klassenstruktur, die nur Erzeugen einer einzigen Instanz erlaubt.
- Kontrollierter Zugriff auf Instanz d. Klasse

```csharp
public class Logger
{
	// statische Instanz - wird nur einmal erstellt
	private readonly static Logger _instance = new Logger();
	
	// privater Konstruktor - verhindert Erstellung v. Instanzen v. außen
	private Logger() { }

	// Klassenmethode für globalen Zugriff
	public static Logger GetInstance() { return _instance; }

	// beliebige Methode über Instanz aufrufbar
	public void Log(string message) { Console.WriteLine(message); }
}
```

Zugriff:

```csharp
// Nicht mehr möglich (kann keine Instanz erstellt werden):
Singleton sin1 = new Singleton();

// Möglich (es wird nur eine Instanz erstellt und immer wieder genommen):
var instance = Singleton.GetInstance();  
instance.SendMessage("hello world!");
```

---
## # Factory

Entkoppelung zw. Objektverarbeitung & Objekterzeugung.
Anstatt in Client-Klasse direkt Objekte zu erstellen - Delegierung zu Erstellung an Factory.
- Vereinfacht Erstellung von komplexen Objekten
- Wiederverwendbarkeit
- einfach erweiterbar
ODER: Eine Methode/Klasse, die Objekte eines bestimmten Typs erzeugt & zurückgibt.

```csharp
// Factory class
public class DuckFactory : IDuckFactory  
{  
    public IQuackBehaviour CreateRedheadDuck() => new RedheadDuck();  
    public IQuackBehaviour CreateMarbledDuck() => new MarbledDuck();  
    public IQuackBehaviour CreateRubberDuck() => new RubberDuck();  
}  

public interface IQuackBehaviour  
{  
    string Quack();  
}  
  
// Factory class interface  
public interface IDuckFactory  
{  
    IQuackBehaviour CreateRedheadDuck();  
    IQuackBehaviour CreateMarbledDuck();  
    IQuackBehaviour CreateRubberDuck();  
}  
  
public class RedheadDuck : IQuackBehaviour  
{  
    public string Quack() => "quack quack";  
}  
  
public class MarbledDuck : IQuackBehaviour  
{  
    public string Quack() => "qua qua qua";  
}  
  
public class RubberDuck : IQuackBehaviour  
{  
    public string Quack() => "squeeze";  
}
```

Aufruf unserer Factory:

```csharp
IDuckFactory factory1 = new DuckFactory();  
var redheadDuck = factory1.CreateRedheadDuck(); 

Console.WriteLine(redheadDuck.Quack());  
Console.WriteLine(factory1.CreateMarbledDuck().Quack());
// Output: quack quack; qua qua qua
```

---
### # Adapter

Vermittler zw. mehreren Klassen.
Dient als "Übersetzer".
Schnittstelle eines Objektes kann zur Laufzeit geändert werden.

Bsp.: Gans kann nicht quacken
- mit Adapter - wenn wir sagen "quack", wird durch Adapter zu "honk" übersetzt

```csharp
public interface IQuackBehaviour  
{  
    string Quack();  
}  
  
public interface IHonkBehaviour  
{  
    string Honk();  
}  
  
public class RedheadDuck : IQuackBehaviour  
{  
    public string Quack() => "... quack quack";  
}  
  
public class MarbledDuck : IQuackBehaviour  
{  
    public string Quack() => "... qua qua qua";  
}  
  
public class Goose : IHonkBehaviour  
{  
    public string Honk() => "... honk honk";  
}  
  
public class HonkAdapter(IHonkBehaviour honkBehaviour) : IQuackBehaviour  
{  
    private IHonkBehaviour _honkBehaviour = honkBehaviour;  
  
    public string Quack()  
    {  
        return _honkBehaviour.Honk();  
    }  
}  
  
public static class DuckSimulator  
{  
    public static void Simulate(List<IQuackBehaviour> ducks)  
    {  
        foreach (var duck in ducks)  
        {  
            Console.WriteLine(duck.Quack());  
        }  
    }}
```

Verwendung:

```csharp
List<IQuackBehaviour> ducks = new();  
ducks.Add(new RedheadDuck());  
ducks.Add(new MarbledDuck());  
ducks.Add(new HonkAdapter(new Goose()));  
  
DuckSimulator.Simulate(ducks);
// Output: ... quack quack; ... qua qua qua; ... honk honk
```

Testfrage: Was muss ich machen, um einen Adapter zu implementieren? 
1. Interface (IQuackable) definieren
2. Andere Klasse (Goose) mit anderer Funktionalität & Interface definieren
3. Adapter Klasse erstellen
	1. diese übernimmt zu übersetzendes Interface (IQuackBehaviour)
	2. in Adapter-Class die Methode der ursprünglichen Klasse aufrufen (Goose - Honk()) - muss allerdings an Quack() angepasst werden

Testfrage: Wenn wir das Aufrufen, was machen dann die anderen?
- Die anderen Duck-Arten implementieren direkt das IDuckBehaviour - haben daher deren eigene Quack()
- Nur bei der Goose muss dies konvertiert werden, weil eine Gans nicht quacken kann

---
### # Decorator

Verhalten von Objekten kann zur Laufzeit verändert werden - oft ist das sogar notwendig.
Decorator haben gleichen Datentyp, wie Objekte, die sie dekorieren
- stellvertretend für dekorierendes Objekt 

Bsp.: Wir haben Winkler & hängen ihm Schmuck um (Dekoration)

Bsp.: Ente: mit Decorator - ich mache das, was Ente macht und mit etwas noch dazu.
- Hinzufügen v. zusätzlichen Funktionalitäten

```csharp
public interface IQuackBehaviour  
{  
    string Quack();  
}  
  
public class RedheadDuck : IQuackBehaviour  
{  
    public string Quack() => "... quack quack";  
}  
  
public class MarbledDuck : IQuackBehaviour  
{  
    public string Quack() => "... qua qua qua";  
}  
  
// Decorator - Output  
public class OutputDecorator : IQuackBehaviour  
{  
    private IQuackBehaviour _quackBehaviour;  
  
    public OutputDecorator(IQuackBehaviour quackBehaviour)  
    {  
        this._quackBehaviour = quackBehaviour;  
    }  
  
    public string Quack() => "Output: " + _quackBehaviour.Quack();  
}  
  
// Decorator - Count  
public class QuackCountDecorator : IQuackBehaviour  
{  
    private IQuackBehaviour _quackBehaviour;  
    public static int Counter = 0;  
  
    public QuackCountDecorator(IQuackBehaviour quackBehaviour)  
    {  
        this._quackBehaviour = quackBehaviour;  
    }  
  
    public string Quack()  
    {  
        Counter++;  
        return _quackBehaviour.Quack();  
    }  
}
```

Verwendung:

```csharp
List<IQuackBehaviour> ducks = [];  
  
ducks.Add(  
    new QuackCountDecorator(  
        new OutputDecorator(   
            new RedheadDuck()
            )  
        )        
    )    
);  
  
DuckSimulator.Simulate(ducks);  
  
Console.WriteLine(QuackCountDecorator.Counter);

// Output: Output: mmmmmooooooinnnnn meiiisterrr: ... quack quack; 1
```

---
## # Command

Um Methoden wie Objekt zu verwenden.
Methodenobjekte in Warteschlangen, Logbucheinträge, Auswirkungen v. Methode rückgängig machen.

Pro command gibt es eine Klasse. 

Bsp.: public class MoveUpCommand > public void Process()
- In Bsp. ACommand - sollte ICommand implementieren
- In Bsp. wird ein Stack<> verwendet 

Bsp.: Strg+Z - kann mit Command Pattern implementiert werden.

In diesem Beispiel:
- Fernbedienung interagiert nicht direkt mit Fernseher, sondern über Command-Objekte 
	- Es können neue Commands hinzugefügt werden, ohne Fernbedienung/Fernseher umschreiben zu müssen

```csharp
public class TV  
{  
    public void TurnOn() => Console.WriteLine("TV Turned on");  
    public void TurnOff() => Console.WriteLine("TV Turned off");  
}  
  
public interface ICommand  
{  
    public void Execute(TV tv);  
}  
  
public class TurnOffCommand : ICommand  
{  
    public void Execute(TV tv) => tv.TurnOff();  
}  
  
public class TurnOnCommand : ICommand  
{  
    public void Execute(TV tv) => tv.TurnOn();  
}  
  
public class RemoteControl  
{  
    private TV _Tv { get; set; }  
    private ICommand Switch1;  
    private ICommand Switch2;  
  
    public RemoteControl(TV tv)  
    {  
        _Tv = tv;  
    }  
  
    public void SetSwitch1(ICommand command) => Switch1 = command;  
    public void SetSwitch2(ICommand command) => Switch2 = command;  
  
    public void PressSwitch1() => Switch1.Execute(_Tv);  
    public void PressSwitch2() => Switch2.Execute(_Tv);  
}
```

Verwendung:

```csharp
var myFernseher = new TV();  
var Fernbedienung = new RemoteControl(myFernseher);  
Fernbedienung.SetSwitch1(new TurnOnCommand());  
Fernbedienung.SetSwitch2(new TurnOffCommand());  
  
Fernbedienung.PressSwitch1();  
Fernbedienung.PressSwitch2();
// Output: TV Turned on; TV Turned off
```

---
### # Strategy

Ermöglicht Verhaltensänderung eines Objektes zur Laufzeit.
Verhalten von Klasse - in eigene Klasse. Immer eine Entscheidung - entweder das eine/andere

Bsp.: Bezahlvorgang - kann beliebige Klasse für Bezahlen verwenden
- Bezahlvorgang = Strategie; kann bei jedem Vorgang beliebig gewählt werden

```csharp
public interface IPaymentStrategy  
{  
    public void Pay(int amount);  
}  
  
public class CreditCardStrategy : IPaymentStrategy  
{  
    private string _cardNumber;  
    private string _name;  
  
    public CreditCardStrategy(string name, string cardNumber)  
    {  
        _name = name;  
        _cardNumber = cardNumber;  
    }  
  
    public void Pay(int amount)  
    {  
        Console.WriteLine($"Bezahlt mit Kreditkarte: {_name}, {_cardNumber}, Betrag: {amount}");  
    }  
}  
  
public class PaypalStrategy : IPaymentStrategy  
{  
    private string _email;  
    private string _password;  
  
    public PaypalStrategy(string email, string password)  
    {  
        _email = email;  
        _password = password;  
    }  
  
    public void Pay(int amount)  
    {  
        Console.WriteLine($"Bezahlt mit PayPal: {_email}, Betrag: {amount}");  
    }  
}  
  
public class Item  
{  
    public string UpcCode { get; }  
    public int Price { get; }  
  
    public Item(string upc, int price)  
    {  
        UpcCode = upc;  
        Price = price;  
    }  
}  
  
public class ShoppingCart  
{  
    private List<Item> _items = new();  
    private IPaymentStrategy _paymentMethod { get; set; }  
  
    public void AddItem(Item item)  
    {  
        _items.Add(item);  
    }  
  
    private int CalculateTotal()  
    {  
        int sum = 0;  
        foreach (var item in _items)  
        {  
            sum += item.Price;  
        }  
        return sum;  
    }  
  
    public void SetPaymentMethod(IPaymentStrategy paymentMethod)  
    {  
        _paymentMethod = paymentMethod;  
    }  
  
    public void Pay()  
    {  
        var amount = CalculateTotal();  
        _paymentMethod.Pay(amount);  
    }  
}
```

Verwendung:

```csharp
var cart = new ShoppingCart();  
cart.AddItem(new Item("001", 100));  
cart.AddItem(new Item("002", 200));  
  
cart.SetPaymentMethod(new PaypalStrategy("Max Mustermann", "123456789"));  
cart.Pay();
// Output: Bezahlt mit PayPal: Max Mustermann, Betrag: 300
```
