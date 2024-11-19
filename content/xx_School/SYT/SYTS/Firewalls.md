#Definitions 

---
## # Definition

Urspünge: Brandschutzmauer zwischen Holzhäusern
- Schutz vor Häuserübergreifenden Bränden - von einem zum Nächsten eindämmen

---
## # Firewall Technologien

- Komponenten zum Schutz v. Subnetzen & Netzsegmenten
	- _regelbasiertes Filtern v. Datenpaketen_
- keine volllständige Isolierung
	- kontrollierter Datenverkehr zw. Innen (LAN) & Außen (WAN) 

### # Annahme

- keine klare Grenze zw. "Innen" & "Außen"
- zentrale Kontrollpunkte
- sämtlicher Datenverkehr muss die Kontrollpunkte passieren

### # Realität

- Mobilität, Dynamik v. Endgeräten
	- Wo verläuft die Grenze?
- unmengen private Geräten (Handy, ...)
	- gestaffelte Kontrollen & Kontrolldomänen sind wichtig!

---
## # Technische Umsetzung

Firewall besteht aus 1/mehreren *Soft- und/oder Hardwarekomponenten*
- untersch. Firewalltypen - verschiedene Komplexität/Aufgaben
- Kombination unterschiedlicher Firewalltypen = Firewall-Topologien
- Firewall realisiert Sicherheits-Policy
	- Zugriffsrichtlinien (wer darf welche Pakete erhalten)
	- Auditing (Protokollierungsanforderungen)
	- Authentifikation v. Usern, PC

---
## # Technische Ebene

- Policy - durch Filterregeln definiert
- einfache Sprache zur Definition v. Filterregeln
- Filterregeln können 
	- zustandsbehaftet
	- dynamisch
	- zustandslos (einfach)
- Ein und ausgehende Pakete - gegen definierte Regeln geprüft (Sernder/Empfänger - IP-Adresse)

---
## # 2 Firewall-Klassen

### # Paketfilter

Trennen v. WAN mit LAN. Alles läuft über zentralen Kontrollpunkt. Definierten policies ("Firewall-Regeln") greifen.

![[Pasted image 20241021095029.png]]

### # stateless Paketfilter (älter)

simple, "dummer" Typ.
Jedes Netzwerk-Paket wird unabhängig von anderen behandelt. Merkt sich keine Zustände/Verbindungen zw. Sender & Empfänger

- auf Netzwerk/Transportebene - Layer 3/4
- Datenpakete gefiltert nach
	- Sender/Empfänger IP-Adresse
	- Ports
	- Protokoll (TCP, UDP, ICMP)
	- Paket-Größe (DoS-Angriffe erkennbar)

Probleme
- nur einzelne Pakete gefiltert - keine zusammenhängenden Ströme
- keine Zustandsinformationen - Callback-Problem

---
### # stateful Paketfilter

erweitert, "neuerer" Typ 

- Verfolgung v. Verbindungsstatus
- Header & Inhalt des Datenpaketes wird analysiert
- Filtern nach Kontext - Antwort/Anfrage an Teil von fragmentierten Pakets
- Erkennen & Abwehr v. DoS-Angriffen

![[Pasted image 20241021105717.png]]

- Notes: Der Filter merkt sich die in grün angegebenen Informationen
	- Filter erlaubt Kommunikation über Port 3264
	- Filter erlaubt nicht Kommunikation über Port 3265 

Stateful: Probleme
- Überlastung durch Ressourcenspeicherung von DoS-Angriffen
- keine garantierte Verbindung bei UDP - Angreifer gibt sich als valides UDP-Paket aus

---
## # Fazit: Paketfilter

Vorteile
- transparent für Anwendungen (arbeitet auf Netzwerkschicht - keine Anpassung v. Anwendung notwendig)
- effizient & kostengünstig (oft in Router integriert = benötigen keine Zusatz Hard-Software)

Standardempfehlung
- Standard: default deny
- nur notwendige Dienste von außerhalb öffnen = minimale Angriffsfläche

Nutzen
- Abwehr v. simplen DoS-Attacken & Spoofing-Versuchen
- schnelles "Screening" - grobes Filtern von bösartigem Verkehr

----
### # Grenzen v. Paketfiltern

- kann nicht nach spezifischen Benutzern filtern
- Header-Daten für Filterentscheidungen können einfach manipuliert werden (IP-Adressen, Ports)
- Filter nur nach Port, nicht nach Dienst
- bei komplexen Netzwerkstruktur - Filterregeln werden komplex & schlecht wartbar
- erlauben/verbieten nur auf Basis v. Header-Daten

---
### # Vor-/Nachteile v. Paket-Filtern

| +                                                | -                                                       |
| ------------------------------------------------ | ------------------------------------------------------- |
| hohe performance                                 | keine Kenntnisse über transportierte Inhalte            |
| leichte Implementierung                          | IP-Spoofing anfällig                                    |
| leicht skalierbar                                | einige Protokolle liefern Probleme                      |
| unabhängig v. Applikation                        | bei großen Netzen - Filterregeln werden unübersichtlich |
| fast in allen heutigen Routern per default dabei |                                                         |

---
### # Proxy Firewalls

Auch "Application Level Firewall" genannt (wenn an Protokoll angepasst)

Idee: Proxy mit Stellvertreterkonzept
- Proxy-Server agiert als Gateway
- Caching v. URLs

Aufgabe
- Proxy übernimmt Sicherheitsfunktion
	- gegenüber Client - als Server
	- gegenüber Server - als Client

![[Pasted image 20241021144137.png]]

Notes: Proxy sammelt Daten & analysiert Datenstrom, bevor er weitergeleitet wird
- Überprüfung Zulässigkeit 
- Anlegen v. Zustandsinformationen
- Logging

### # Proxies d. Verbindungsebene

"circuit-level" - stellen generische Dienste zur Verfügung
leiten Datenverkehr weiter, ohne Inhalt zu interpretieren
- niedrigere Ebene v. Netzwerkprotokoll

### # Proxies der Anwendungsebene

Proxy-Dienste auf Anwendung zugeschnitten
Analysieren von Datenverkehr & Steuerung

---
### # Problem bei Proxies

1. Transparenz - ursprüngliche Client-Server Verbindung wird durch 2 seperate Verbindungen ersetzt (Client --> Proxy, Proxy --> Server)
2. E2E-Sicherheit - End-To-End Sicherheit leidet
	1. man-in-the-middle attack - Proxy könnte Daten zw. Client & Server lesen/ändern
3. Zeitverzögerung - store-and-forward prinzip

---
### Application-level Gateway

Diese gateways arbeiten auf 7. Schicht von OSI-Modell (Anwendungsschicht) & bieten Kontrollfunktionen, speziell auf Anforderungen v. Anwendungen zugeschnitten



