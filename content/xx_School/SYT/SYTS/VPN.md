#Definitions 

---

"Virtual Private Network"

1. Authenzität - nur mit Authentifizierung möglich, um VPN nutzen zu können
2. Vertraulichkeit (verschlüsselte Daten)
3. Integrität

---
## # 3 Modi

1. "Site-to-site" (Standort1 zu Standort2)
	1. gateway-to-gateway
2. "End-to-site" (PC1 zu Firma)
	1. Client connects to company network (gateway)
	2. client-to-gateway
3. "End-to-end" (PC1 direkt zu Fileserver)
	1. Example: connecting to a fileserver
	2. client-to-client

---
## # VPN-Tunnel

tunneln = Pakete in anderes Protokoll verpacken (Daten können am Übertragungsweg nicht gesehen werden, erst wenn Nachricht bei Empfänger ist)
- fügt neuen Header mit zusätzlichen Informationen hinzu

---
## # IPsec

"Internet Protocol Security" - häufig bei VPNs eingesetzt 

hat 2 Betriebsmodi:
1. Transport-Modus: Verschlüsselung von nur Dateninhalt
2. Tunnel-Modus: Verschlüsselung von kompletten IP-Paket in neues IP-Paket mit zusätzlichem Header



