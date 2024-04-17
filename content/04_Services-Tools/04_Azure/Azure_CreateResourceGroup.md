#Services 

---
## # Creating Resource Group

Resource Group> Create Resource Group > Subscription-Resource-Group-Region > Next > Next >  Create

In Resource Group > Create .... > Marketplace > Azure SQL > Create > SQL databases

** > Database details **

Database name > Server - Create new > Create SQL Database > Review + Create > Create
- Server name > Location > Authentication method (default: you need a microsoft acc for authentication) - we use "Use both SQL and Microsoft Entra authentication" > server admin login > _ItpDbAdmin: root1234/_ > Ok 
	- set admin > clemens johannes schmid (microsoft account) > select

---
## # What is Azure SQL?

Microsoft SQL-Server, which is customized for using it in Azure. 

---

	itpss2024 > networking > public access > selected networks > Azure Dienste und Ressourcen haken setzen > save
itpss2024 > overview > db > Settings > connection strings

---
### # ASP .NET Core - add DB

```json
{  
  "Logging": {  
    "LogLevel": {  
      "Default": "Information",  
      "Microsoft.AspNetCore": "Warning"  
    }  
  },  
  "AllowedHosts": "*",  
  "ConnectionStrings": {  
    "Default" : "Server=dbname.database.windows.net; User=username; Password=password/; Database=dbname"  
  }  
}
```