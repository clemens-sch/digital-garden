#DataBases 

---
## # Transaktion ?

 > Wikipedia: "Folge von Programmschritten, die eine logische Einheit sind."

Eine Transaktion muss immer erfolgreich abgeschlossen werden.
- Fehler - alles rückgemacht werden (Rollback)
**Transaktion - ganz oder gar nicht**

Rückgängig machen: neue Transaktion starten & Gegenteil (Gegenbuchung) machen.

---
## # ACID-Prinzip

`Atomarität Konsistenz Isolation Dauerhaftigkeit`

Muss bei Ausführung von Transaktionen die **ACID**-Eigenschaften garantieren.

---
## # Bsp.: Implementierung ACID-Prinzip

Schüler mit zugehöriger Klasse 4CHIT sollen zu 5CHIT umgeändert werden. Kann erst als abgeschlossen und vollständig bezeichnet werden, bis alle Schüler die Klasse 5CHIT zugeordnet haben.

---
## # "Lost Update"

Vergleichbar mit Race-Condition.
Kann bei **Read** + **Update** + **Commit** passieren. Wenn beide gleichzeitig etwas ändern.

Auf Db-Ebene deadlock verhindern:
- "S-lock"/"R-lock" ("Shared-lock") - READ
- "X-lock" ("eXclusive-lock") - WRITE
- "A-lock" ("Additional-lock") - verhindert generelle Ausführung für anderen
- "U-lock" ("Update-lock")

---
## # Sperrverfahren ?

In Datenbanksystem eingesetzt, um ACID-Prinzip zu garantieren.

### # Kompatibilitätsmatrix

the locks I have: →
the locks I want: v

---
### # SX-Matrix

Example: Library (Db) & Books (Data).
- Reading a book = S-lock (more people can read the same book at the same time)
- Lend a book = X-lock (only one can lend a book & others can't lend it at same time)

![[Pasted image 20240918115946.png]]

---
### # SAX-Matrix

Extends lock with A-lock or U-lock. 

Example: Library & Books
- Reading a book. More people can read same book simultainously (S-lock).
- ... 

![[Pasted image 20240918120025.png]]

---
### # SUX-Matrix

![[Pasted image 20240918120450.png]]

---
## # SQL - FOR UPDATE

Verwendet SAX-Kompatibilitätsmatrix.

Ich lese Datenbestände, im vollen Wissen, dass ich sie nachher editieren werde. (halber X-lock)
Es können nicht 2 Programme gleichzeitig was an den Daten ändern.

```sql
SELECT ... FOR UPDATE
```

---
## # Buffer

Sind wie Transaktions-Logs.
Jede Db führt Transaktions-Buffer für Backup.

### # "REDO"-Buffer

Wie Backup. Zu dieser Zeit ist dieser Datensatz zu gewissem Inhalt gespeichert.

### # "UNDO"-Buffer

Bei Änderung. Wird mit alten Wert befüllt.
Ist "Ring-Buffer" - wenn voll: Überschreiben ältesten Werte

---
## # Historische Daten anzeigen

Durch Buffer - Anzeige von vergangen Daten

- Oracle = Flashback
- Postgres = MVCC
- MySQL = nix
- MSSQL = nix

---
## # Isolation Levels

1. Read Uncommited - 
2. Read Commited - 
3. Repeatable Read - 
4. Serializable - Verbieten von parellelen Schreiboperationen
	1. kein X-lock, kein S-lock


