#Software-Engineering 

---
## # Kapitel 6

- Unterprogramme (Methoden) - Programme auf unterschiedliche Einheiten aufteilen & unterschiedlich aufrufen
	- Kapselung - abgeschlossen von der Außenwelt
	- Wartbarkeit sinkt nicht durch Unterprogramme
	- Container

---
### # Objektorientierung - Zusammenfassen von Strukturen & Unterprogrammen

#### # Klasse

Bauplan für Objekt - stellt Funktionen zur Verfügung
Kapselt Daten (Properties) & Verhalten (Methoden)

Wodurch ist ein Objekt in der OOP beschrieben --> Properties, Zustand, Methode.
Was sind 4 zentralen der OOP --> Kapselung, Vererbung, Polymorphismus (), Abstraktion
#### # Objekt



---
### # Schichtenmodelle

Ein Programm in mehrere Schichten zerlegen.

Komplexes Problem in unterschiedliche Teilprobleme zerlegen.

---
### # Komponenten

Unabhängige Teile eines Programms.
Sagt wenig über Datenaustausch zwischen Komponenten aus.

---
### # (Micro)Services

Komponentenorientiert - Komponenten laufen als eigene Prozesse.

---
Strukturierung:
1. Softwareanwendungen>Services
2. Komponenten>Schichten>Klassen
3. Klassen>Methoden

---

**KISS** - "Keep It Simple Stupid"

---
## Kapitel 7 - Metrik

Funktion, die Eigenschaft in einer Zahl abbildet.

### # Qualitätsmetriken

- **Koppelung** - wie hängen unteschiedliche Softwareeinheiten zusammen (bsp.: 2 Klassen)
	- Schwache/Starke Interaktions/Vererbungs-Koppelung
	- Eine Klasse sollte nie eine andere Klasse verwenden! = Interfaces, [[SE_Dependency-Injection]], ...
- **Kohäsion** -  (bsp.: Klasse Taschenrechner mit Methode InitiateHttpConnection - macht keinen Sinn --> gehört nicht in Klasse)
	- innerer Zusammenhalt soll hoch sein - Dinge, die zusammengehören, sollten zusammen sein
	- schwache/starke Kohäsion - klassenkohäsion/... 

---
### # Vererbungskoppelung


