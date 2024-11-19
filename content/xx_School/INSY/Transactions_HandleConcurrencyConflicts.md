[Microsoft: Handle Concurrency Conflicts](https://learn.microsoft.com/en-us/ef/core/saving/concurrency?tabs=data-annotations)

#Definitions  

---
## # Zeitgleiche Änderungen verhindern

In a program, concurrent modifications should be prevented so that two different clients don’t overwrite a value. If both were to change the value simultaneously, data loss would occur.

---
## # Optimistic Concurrency

- verhindert unabsichtliche Datenüberschreibungen
- keine locks = verbessert Systemleistung & Systemreaktionszeit

Vermeiden von Lost-Update Problemen.
Vorgehen: EF Core inkludiert alle columns in ein SQL WHERE statement mit UPDATE

---
### # Timestamp

Von der Datenbank verwaltet. 
Bei Änderungen wird die Version automatisch geändert.

```csharp
public class Sal
{
	public int Id {get;set;}
	public? string Name {get;set;}

	[Timestamp]
	// Concurrent-Token
	public byte[] Version {get;set;}
}
```

---
### # ConcurrencyCheck

Von App-Code verwaltet.
Version ist hierbei ein GUID-Wert, der den aktuellen Zustand des Datensatzes verfolgt. Wenn nun irgend ein Wert von Sal geupdatet wird, wird ein neuer GUID generiert und zugewiesen.
- Symbolisiert eine Änderung von Datensatz
- wenn anderer Client versucht denselben Datensatz zu aktualisieren, wenn GUID bereits geändert wurde, wird Fehler ausgelöst, weil ursprünglicher GUID stimmt nicht mehr überein

```csharp
public class Sal
{
	public int Id {get;set;}
	public? string Name {get;set;}

	[ConcurrencyCheck]
	// Concurrent-Token
	public Guid Version {get;set;}
}
```

```csharp
private void Update() 
{ 
	sal.version = Guid.NewGuid(); // Generierung eines neuen GUIDs
	pgC.SalData.Update(sal); // Aktualisierung des Datensatzes 
	pgC.SaveChanges(); // Speichern der Änderungen 
}
```

Now, if 2 Clients want to change the value at the same time, there will be an `DbUpdateConcurrencyException` thrown. So the row can't be overwritten.

```sql
UPDATE Tab 
	SET F = "...",
	    L = "...",
	    Sal = ...
	WHERE Id = 27
	AND Version = "..."
```

---
### # Datagrip - Transaktionskontrolle Manual

WICHTIG zu wissen: In Datagrip ist TX: auto standard. TX: manual gibt mehr Kontrolle über den Verlauf einer Transaktion.
- dadurch kann man ein COMMIT oder ein ROLLBACK ausführen.
- im manual Mode wird allerdings ein COMMIT/ROLLBACK benötigt um die Transaktion zu schließen

- Transaktionskontrolle auf "Manual" stellen
- starten v. 2 Konsolen in DG
- updaten eines Wertes in der 1. Konsole
- updaten des selben Wertes in 2. Konsole
	- gerät in Warteschlange - weil Datensatz in 1. Konsole gesperrt ist
		- muss daher warten, bis 1. Konsole fertig ist (wartet bis COMMIT/ROLLBACK ausgeführt wird = Sperre aufgehoben)

---
> !!!
## # Pessimistic Concurrency

Datenbank verwendet Sperren ("locks") um gleichzeitigen Zugriff auf Daten zu regeln.

| Operation            | Bezeichnung                     |
| -------------------- | ------------------------------- |
| Lesen von Daten      | S-lock (shared-lock)            |
| Schreiben von Daten  | X-lock (exclusive-lock)         |
| I/A/Update von Daten | U-lock (update-lock) / A/I-lock |
Update-lock = SELECT ... FOR UPDATE

---
## # ACID / DE: "AKID"

erwünschte Eigenschaften von Transaktionen in DBMS

- Atomicy = Abgeschlossenheit 
	- Transaktion ist Folge von Datenbank-Operationen. ALLES-ODER-NICHTS Eigenschaft.
	- Sollte nicht vollständig abgeschlossen sein = Rollback ausgeführt,
- Consistency = Konsistenzerhaltung
	- Nach Beenden der Transaktion - Datenbank soll nachher genauso wie vor Operation einen konsistenten Zustand haben.
- isolation = Isolation (Abgrenzung)
	- üblicherweise durch Sperrverfahren
	- verhindert/schränkt ein, dass sich transaktionen gegenseitig beeinflussen
- durability = Dauerhaftigkeit
	- Daten bleiben nach Abschluss der Transaktion dauerhaft in Datenbank
		- auch nach Hardware/Softwareausfall
		- "Transaktionslog" erlaubt nach Systemausfall alle Schreib-Operationen in DB auszuführen

---
## # Lost Update

Fehler, der bei mehreren parallelen Schreibzugriffen auf eine gemeinsam genutzte Information auftreten kann. 
Bsp.: Könnte eine Änderung direkt von einer anderen Transatkion üebrschrieben werden.

### # Umgehen v. Lost Update

Einführung Sperrmechanismen für Umgehen des Problems (viele Betriebssysteme/DBs verwenden diese)
- s-lock (alle immer lesezugriff)
- x-lock (nur einer schreibzugriff - währenddessen kein anderer lesezugriff)

![[Pasted image 20241014161749.png]]

---
## # Isolation Levels

### # RR - Repeatable Read

RR - oft Lösung zu Lost-Update Problem. 

Daten sind "wiederholbar".
Während Transaktion gelesene Werte bleiben erhalten ("wiederholbar") - nicht von anderen Transaktionen geändert
- kann Deadlock passieren (2 Transaktionen versuchen gleichzeitig s-locks in x-locks zu konvertieren - beide warten auf jeweils anderen)

![[Pasted image 20241014182825.png]]

---
## # Sperre - Postgres DB

> btw. sau unnedigs bsp vom <mark style="background: #D2B3FFA6;">macho</mark> owa jo der <mark style="background: #D2B3FFA6;">hund</mark> kennt si ned moi aus oida

sew setzt x-lock = andere user können keine s-locks/x-locks setzen

---
## # Inconsistent Analysis Problem

Auch "Incorrect summary problem".
Basierend auf gleichzeitigene Änderungen an gleichen Daten.

Bsp.: Berechnung der Summe wird inkonsistent, weil T1 einige Werte vor & andere nach Änderungen von T2 liest (Kombination alter mit neuer Werte)
- =185 - T1 nimmt **alten** Wert v. B aber **neuen** Wert von C.

![[Pasted image 20241014193351.png]]

---
## # Dirty Read

"Schreib-Lese-Konflikt" - wenn 2 Transaktionen laufen und eine Transaktion von der anderen geschriebene/geänderte Daten liest (diese Daten sind noch nicht COMMITTED).
<mark style="background: #D2B3FFA6;">WICHTIG</mark> Ist nicht stabil & könnte zurückgenommen werden
- inkonsistent

Macho ~ "ungeschützter Datenverkehr"

### # Lösung v. Dirty Read

setzen eines x-locks bei Transaktion A
- nun kann Transaktion B nicht mal mehr einen s-lock setzen - muss bis zu COMMIT/ROLLBACK warten
- = Transaktion 2 wird automatisch serialisiert

---
### # Beispiel: Dirty Read (ohne x-lock)

T1: 
- liest Balance
- setzt Balance neu

T2:
- liest den neuen, aber möglicherweise noch nicht stabilen Wert

![[Pasted image 20241014212843.png]]

![[Pasted image 20241014212852.png]]
![[Pasted image 20241014212905.png]]

---
## # Isolation Levels

| Level           | Definition                |
| --------------- | ------------------------- |
| Serializable    | - highest isolation level |
| Repeatable Read | -                         |

---
## # Commit / Rollback

Transaktionslogs
- Verfolgen von Datenänderungen
- Wichtig für Wiederherstellung & Konsistenz v. Daten

Bsp.: Überweisung 10€ v. Konto C --> Konto A
- **Aktuelle Kontostände:**
    
    - **Konto A:** 100€
    - **Konto B:** 50€
    - **Konto C:** 25€
- **Transaktion:**
    
    - **Zuerst** wird Konto C um 10€ verringert: C=C−10C = C - 10C=C−10 (also C=25−10=15C = 25 - 10 = 15C=25−10=15)
    - **Dann** wird Konto A um 10€ erhöht: A=A+10A = A + 10A=A+10 (also A=100+10=110A = 100 + 10 = 110A=100+10=110)

