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

Auf Db-Ebene deadlock verhindern. 
- "S-lock"/"R-lock" ("Shared-lock") - READ
- "X-lock" ("eXclusive-lock") - WRITE
- "A-lock" - verhindert generelle Ausführung für anderen
- "U-lock" ("Update-lock")

---
## # Sperrverfahren

SX: hier kann deadlock passieren.

![[Pasted image 20240918115946.png]]

SAX: Schreiber wird nach hinten verschoben
- kann erst geschrieben werden, wenn keiner mehr liest

![[Pasted image 20240918120025.png]]

SUX: kann nur schreiben, wenn einziger ist

![[Pasted image 20240918120450.png]]

---
## # SQL - FOR UPDATE

Verwendet SAX-Kompatibilitätsmatrix.

Ich lese Datenbestände, im vollen Wissen, dass ich sie nachher editieren werde. (halber X-lock)
Es können nicht 2 Programme gleichzeitig was an den Daten ändern.

```sql
SELECT ... FOR UPDATE
```


