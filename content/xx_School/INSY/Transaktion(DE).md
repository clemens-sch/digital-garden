#DataBases 

---
## # Transaktion ?

Folge von Programmschritten, die eine logische Einheit sind.

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
## # Lost Update

Vergleichbar mit Race-Condition.

---


