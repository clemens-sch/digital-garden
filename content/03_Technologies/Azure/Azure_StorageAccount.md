#Technologies 

---
## # Resource Group erstellen

- Resouce Group erstellen
	1. Basics
		1. Subscription: Azure for Students
		2. Name: SYTD5C2024
		3. Region: (Europe) West Europe
	2. Tags
	3. Review + create
		1. Create

---
## # Service erstellen

In Resource Group
1. Overview
	1. Service erstellen: Create
		1. Basics
			1. Marketplace: Storage Account > Microsoft: Create
			2. Resouce Group: SYTD5C2024
			3. Storage account name: sytd5cstrg2024schmid
			4. Region: (Europe) West Europe
			5. Primary Service: Azure Blob Storage or Azure Data Lake Storage Gen 2
			6. Performance: Standard
			7. Redundancy: Locally-redundant storage (LRS)
		2. Advanced (Security Settings)
		3. Networking
			1. Enable public access from all networks
		4. Data Protection
			1. alle Haken weg
		5. Encryption
		6. Tags
		7. Review + Create (Zusammenfassung)
			1. Create

"Your deployment is complete"

---
### # Zu Storage Acc navigieren

Resource Groups > Resource Group heraussuchen > Storage Account heraussuchen

---
## # Storage Account ? 

1. Overview
2. ...
3. Data management
	1. Static Website
		1. Enabled - Wenn nun eine Website hochgeladen wird, kann diese direkt vom Storage Account ausgelierfert werden (Mini-Webserver)
4. Storage browser - für gespeicherte Files
	1. Blob containers (Binary Large Object) - ist nicht wirklich ein Filesystem
	   Verw.: für Dateiablage von großen Files
		1. $logs
		2. $web > upload
			1. .html file auswählen > upload
		3. Add container
			1. Name: mycontainer > create
	2. Queues - Schmalspurvariante von RabbitMq
		1. Add Queue
			1. Name: myqueue 
	3. Tables - Schmalspurvariante von Db
		1. Add table
			1. Name: mytable
				1. Add entity
					1. PartitionKey
					2. RowKey
					3. Add property: name String clemens

---
### # Connection String

1. Overview
2. ...
3. Security Networking
	1. Access keys
