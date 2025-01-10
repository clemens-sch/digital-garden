#school 

---
## # Stufen d. Automatisierung
### # Vordefinierte Elemente

1. BLAU: Vordefinierte Elemente

---
### # Konfigurationselemente

1. GRÜN: Erweiterung um Konfigurationselemente
   Baut auf vordefinierten Elementen auf
	1. Patient
	2. Therapist_Schedule
	3. Status

---
### # Automatisierung

3. GELB: Automatisierungstabelle
	1. PatientHasTherapy

![[DA_Buch-Diagramme 2024-12-19 17.48.30.excalidraw]]

---
## # Ablauf: Automatisierung

### # Patient Erstellung & Zuordnung

1. Durch **TherapyPackage** wird dem Patienten fix ein Paket für seine Therapie zugeordnet.
   Das ganze MUSS innerhalb von **Arrivaldate bis Departuredate** so EFFIZIENT wie möglich abgearbeitet werden
   Wichtig: In jedem TherapyPackage muss die Therapie "Abschlussgespräch enthalten sein"

## # Therapy Auflösen d. Ressourcen

2. Über das **TherapyPackage** wird die **Anzahl d. jeweiligen Therapien** aufgelöst.
   Eine Therapie verweist weiters wieder auf ein **Service** wie bsp.weise "Massage" (Kategorie d. Therapie).
   Eine Therapie benötigt außerdem eine spezifische **Qualifikation** bsp.weise "Physio".
   - Diese **Qualifikation** wird einem **Therapeuten** fest zugeordnet
   Eine Therapie benötigt außerdem einen Raum, in dem alles stattfindet.
   - In diesem **Raum** KÖNNEN sich **Hilfsmittel** befinden (der Raum selbst kann bereits als Hilfsmittel fungieren)

## # Einteilung v. verfügbaren Ressourcen

3. Dem **Therapeut** mit der jeweiligen **Qualifikation** wird nun für die **StartTime bis zur EndTime** ein gewisser **Service** zugeordnet.
   Bsp.weise: Therapeut Hans Huber macht am 01.01.2025 von 10:00 Uhr bis 12:00 Uhr den Service "Vortrag".

## # Automatisierung

Im Frontend wird nun ein Button mit "Plan erstellen" verfügbar sein.
- Beim Drücken des Buttons kommt nochmals eine Übersicht, was alles erstellt/bereitgestellt wird.
- Nun werden basierend auf den erstellten Patienten und zugeteilten Dienstplänen der Therapeuten alle Therapien & Pläne erstellt

4. Anhand der angegebenen Daten kann die Automatisierungslösung nun die fertigen Therapien ausspucken und auswerten.
   Die **Therapie** hat nun einen **Patienten** zugeordnet
   Die **Therapie** hat nun einen **Tag + StartTime bis EndTime**
   Die **Therapie** hat nun einen **Raum** zugeordnet
   Die **Therapie** hat nun ein **Hilfsmittel** zugeordnet
   Die **Therapie** hat nun einen **Therapeuten** zugeordnet
   Die **Therapie** hat einen **Status**, der laufend aktualisiert werden muss
   - Bsp.weise: "Geplant", "Abschlossen", ...

