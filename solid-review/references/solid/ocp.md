## Open/Closed Principle (OCP)

> "Software artifacts should be open for extension, but closed for modification." — Bertrand Meyer

You should be able to **add new behavior** to a system without **modifying existing code**. Existing code should be stable; new functionality is added through extension (e.g., new classes, new functions, new types).

### Why it matters
- Every time you modify existing, working code, you risk introducing bugs.
- OCP protects stability: existing code remains untouched while the system grows.

### Guidelines
- **Prefer software design that allows adding types or operations without modifying existing code.**
- Use **polymorphism** (virtual functions, interfaces, protocols) instead of switch/case on type enums.
- Use **type erasure / generic programming** when you want value semantics and decoupling from inheritance hierarchies.
- Template/generic approaches (like `std::copy`) are naturally open-closed: they work for any type satisfying the required concept without modification.

### Procedural vs. Object-Oriented tradeoff
| Approach | Easy to add shapes | Easy to add operations | SRP adherence |
|---|---|---|---|
| Enum/Switch | ✗ (must modify switch) | ✓ (add new function) | Medium |
| OOP/Virtual | ✓ (add new class) | ✗ (must modify interface) | Low |
| Type Erasure | ✓ | ✗ (but decoupled) | High |
| Visitor | ✗ | ✓ | High |

There is no perfect solution — choose based on which axis (types or operations) changes more often.

### Signs of violation
- Adding a new type requires touching multiple `switch` or `if/elif` chains.
- Adding a new feature requires modifying core abstractions.
