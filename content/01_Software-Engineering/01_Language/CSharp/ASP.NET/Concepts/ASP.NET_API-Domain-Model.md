#Software-Engineering 

---
## # Why Layers ?

Also see [[CSharp_EF-Core-Definition]] (Layers / Tiers)

Organizing code into layers is a common practice to promote maintainability, scalability & separation of concerns. 
The use of layers like API, Domain, Model helps in structuring the app.

---
## # Breakdown

![[Pasted image 20240303194442.png]]
### # API Layer

Also see [[ASP.NET_ApiController]]

This layer is responsible for handling HTTP-requests & responses. It includes controlling, routing & the logical requirements for processing requests.
In ASP.NET Core, this layer often defines some "endpoints", that handle incoming requests and return responses.

