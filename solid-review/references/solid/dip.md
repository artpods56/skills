## Dependency Inversion Principle (DIP)

> "The most flexible systems are those in which source code dependencies refer only to abstractions, not to concretions." — Robert C. Martin

High-level policy should not depend on low-level details. **Both should depend on abstractions.** Abstractions should not depend on details — details depend on abstractions.

### The two rules
1. **High-level modules should not depend on low-level modules.** Both should depend on abstractions.
2. **Abstractions should not depend on details.** Details should depend on abstractions.

### Why it matters
- When high-level code directly depends on low-level implementations, changes to details (database, HTTP client, file system) ripple into business logic.
- Inverting the dependency means high-level code **owns the interface it needs** — it is in control of its own abstractions.

### The MVC example
In a typical MVC architecture:
- The **Controller** depends on abstractions (interfaces) defined at the architectural boundary.
- The **View** depends on the same abstractions.
- The **Model** implements those abstractions.
- Dependency arrows point **inward** toward the model, never outward.

### Guidelines
- **Prefer to depend on abstractions** (abstract classes, interfaces, protocols, concepts) instead of concrete types.
- Define the interface in the **high-level module**, not the low-level one.
- Use dependency injection to provide concrete implementations at composition time.
- In generic/template programming, the algorithm defines the concept (abstraction) — the types adapt to it (e.g., `std::copy` defines what it needs; types satisfy those requirements).

### Signs of violation
- `import` of a concrete class deep inside business logic.
- `new` or direct instantiation of infrastructure inside domain code.
- Difficulty swapping out a database, logger, or HTTP client without touching business rules.
