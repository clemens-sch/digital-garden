#Software-Engineering 

---
## # Sinn der Übung

möglichst geringe Koppelung
möglichst hohe Kohäsion

---
## # geringe Koppelung durch

1. Interfaces (IConvoy)
2. Dependency Injection (builder.Services.AddSingleton<IConvoy, Convoy>())

---
## # hohe Kohäsion durch

Bsp.: ConvoyFacade darf nicht AssignToSlot method beeinhalten.

