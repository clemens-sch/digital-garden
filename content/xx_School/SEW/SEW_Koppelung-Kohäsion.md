#Software-Engineering 

---
## # Koppelung

Maß für Abhängigkeit unter Softwareelementen. 
>Anstreben von: Loose Coupling

Es gibt 2 Stufen von Koppelung:

1. Tight Coupling
	- "starke Koppelung"
	  tritt auf, wenn eine `Klasse direkt auf Implementierungen anderer Klassen zugreift` = _Wartbarkeit wird beeinträchtigt_
2. Loose Coupling
	- "schwache Koppelung"
	  Klassen haben nur minimale Kenntnisse zu anderen Klassen
	  Zugriff auf `Funktionalität läuft über Interfaces` 

2 unterschiedliche Arten v. Koppelung:
1. Interaktionskoppelung
	- Maß an Funktionalität eines anderen Objekt, Klasse
	- tritt auf, wenn Objekte einer Klasse, Methoden von Objekten anderer Klassen aufrufen
2. Vererbungskoppelung
	- Ausmaß der Abhängigkeit
	- tritt auf, wenn ein Objekt einer Klasse direkt auf ein anderes Objekt einer Klasse verweist (erbt)

---
## # Kohäsion

Maß für inneren Zusammenhalt eines Softwareelements.
>Anstreben von: Hoher Kohäsion

Definition v. Kohäsion: Klasse sollte nur Methoden/Attribute enthalten, die alle Lösung einer gemeinsamen Aufgabe/Verwantwortungsbereich gehören.

2 unterschiedliche Arten v. Kohäsion:
1. ServiceKohäsion
	- Beschreibung zu `inneren Zusammenhalt einer Methode`
	- Methoden einer Klasse - nur EINE Lösung von Aufgabe/Problem 
2. Klassenkohäsion
	- Beschreibung zu `inneren Zusammenhalt einer Klasse`
	- Verletzung v. Klassenkohäsion - ungenutzte Attribute/Methoden in Klasse

### # Bsp.: Servicekohäsion

```csharp
1 //------------------------------------------
2 // Servicekohaesion - schwache Kohäsion
3 //------------------------------------------
4 class Vector implements Serializable{
5 public int X { get; set; }
6 public int Y { get; set; }
7
8 public float Add(Vector v){
9 this.X += v.X;
10 this.Y += v.Y;
11
12 return Math.SQRT(X * X + Y * Y);
13 }
14
15 }
```