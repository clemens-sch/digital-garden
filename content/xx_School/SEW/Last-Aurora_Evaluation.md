#school 

---
## # Hofbauer, Hofstetter

### # Project-Structure

- Solution: LastAurora 
	- LastAurora - Classes, Interfaces
	- LastAurora - Unit-tests 
	- LastAuroraWeb - UI for convoy-management

---
## # Interfaces
### 1. ICargo Interface

- **Cohesion**: High. Provides a method for describing cargo.
- **Coupling**: Low. Can be implemented by any class without dependencies on specific implementations.
- **Strengths**: Clear purpose and easy to extend with new cargo types.
- **Improvements**: None needed;

### 2. IDamageStrategy Interface

- **Cohesion**: High. It focuses solely on damage application and repair, maintaining a clear responsibility.
- **Coupling**: Low. Like `ICargo`, it's an interface, allowing flexible implementation without tight coupling to other classes.
- **Strengths**: Allows different damage behaviors to be defined easily.
- **Improvements**: None needed;

### 3. IVehicle Interface

- **Cohesion**: High. It provides a clear purpose by requiring a price property for vehicles.
- **Coupling**: Low. It allows various vehicle types to implement the interface without depending on specific classes.
- **Strengths**: Flexibility in defining different vehicle types while ensuring they have a common attribute.
- **Improvements**: None needed;

---
## # Classes
### 4. Addon Class

- **Cohesion**: Moderate to High. It has properties related to an add-on, but it could potentially be split into more specific add-on types.
- **Coupling**: Low.
- **Strengths**: Holds all necessary data for an add-on in one place.
- **Improvements**: None needed;

### 5. Crew Class

- **Cohesion**: High. It focuses on defining crew cargo and describing it.
- **Coupling**: Low. It only implements `ICargo`, keeping it loosely coupled to other classes.
- **Strengths**: Clear and straightforward, fulfilling its purpose effectively.
- **Improvements**: None needed;

### 6. Munition Class

- **Cohesion**: High. It’s dedicated to defining munition cargo with a clear method for description.
- **Coupling**: Low. It refers to the `ICargo` interface without additional dependencies.
- **Strengths**: Specific and well-defined functionality.
- **Improvements**: None needed;

### 7. ChemischeWaffen Class

- **Cohesion**: High. Focuses on chemical weapons, with a clear responsibility.
- **Coupling**: Low. Like other cargo classes, it implements `ICargo`, maintaining loose coupling.
- **Strengths**: Simple and effective in its design.
- **Improvements**: None needed;

### 8. Cargo Class

- **Cohesion**: High. It has properties and behavior of generic cargo.
- **Coupling**: Low. It implements `ICargo` without dependencies on other classes.
- **Strengths**: Flexible for general cargo use.
- **Improvements**: None needed;

### 9. KonvoiFacade Class

- **Cohesion**: Moderate. It handles various tasks related to managing a convoy, which could lead to multiple responsibilities.
- **Coupling**: Moderate. It relies on several vehicle types and their properties.
- **Strengths**: Centralized management of the convoy, making it easier for managing a convoy.
- **Improvements**: Maybe breaking down methods into smaller classes.

### 10. Slot Class

- **Cohesion**: High. It focuses on managing the state and cargo assignments of a slot.
- **Coupling**: Moderate. It relies on `IDamageStrategy` and `ICargo`.
- **Strengths**: Well-structured for handling cargo assignments and status changes.
- **Improvements**: Could provide additional methods for handling slot state transitions.

### 11. StandardDamageStrategy Class

- **Cohesion**: High. Focused on applying and repairing damage to slots.
- **Coupling**: Low. It doesn’t depend on any other classes except for the `Slot` type it operates on.
- **Strengths**: Implements the Strategy Pattern effectively for damage management.
- **Improvements**: None needed; 

### 12. Trailer Class

- **Cohesion**: High. It represents a trailer and its properties, based on `IVehicle`.
- **Coupling**: Low. It's a specific implementation that remains loosely coupled due to the interface.
- **Strengths**: Clear and focused on the trailer's attributes.
- **Improvements**: None needed;

### 13. Truck Class

- **Cohesion**: High. It has truck-related properties while implementing `IVehicle`.
- **Coupling**: Low. Similar to the `Trailer`, it maintains loose coupling through the interface.
- **Strengths**: Clearly defines truck attributes and responsibilities.
- **Improvements**: None needed;

### 14. Upgrade Class

- **Cohesion**: Moderate. It combines functionality related to vehicles, slots, and add-ons, which gets more complex. Tasks need to be defined clearly.
- **Coupling**: Moderate. It interacts directly with `Slot` and `Addon`, which leads to tighter coupling.
- **Strengths**: Provides a mechanism for managing add-ons and slots effectively.
- **Improvements**: Consider breaking it down into smaller components or classes to focus on different responsibilities.

---
## # Conclusion

Most interfaces, such as `ICargo`, `IDamageStrategy`, and `IVehicle`, have high cohesion with low coupling, allowing for clear responsibilities and flexible implementations. 
This design provides maintainability & reusability.

Classes like `Crew`, `Munition`, and `ChemischeWaffen` also have high cohesion, maintaining clear, focused functionalities. 
While the `Addon` class has moderate cohesion, it still is low coupled, which is effective. However, the `KonvoiFacade` and `Upgrade` classes could be improved to make them more focused and reduce their connections to other parts/classes.
