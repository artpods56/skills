## Liskov Substitution Principle (LSP)

> "Subtypes must be substitutable for their base types." — Barbara Liskov

If `S` is a subtype of `T`, then objects of type `T` may be replaced with objects of type `S` without altering any of the desirable properties of the program. Inheritance must be about **behavior**, not just data.

### Formal rules of behavioral subtyping
- **Contravariance** of method arguments in a subtype.
- **Covariance** of return types in a subtype.
- **Preconditions** cannot be strengthened in a subtype.
- **Postconditions** cannot be weakened in a subtype.
- **Invariants** of the supertype must be preserved in a subtype.

### Why it matters
- Violating LSP means callers cannot trust the base type contract — they must know about specific subtypes, defeating the purpose of abstraction.
- It leads to `isinstance()` checks scattered through the codebase.

### The Square/Rectangle problem
- A `Square` is **not** a `Rectangle` in behavioral terms: `Square` constrains `setWidth` to also change height, violating `Rectangle`'s contract.
- **IS-A is about behavior, not about real-world classification.**

### Guidelines
- **Make sure inheritance is about behavior, not about data.**
- **Make sure the contract of base types is adhered to** by all subtypes.
- **Make sure to adhere to the required concept** (in generic/template programming).
- If a subtype throws `NotImplementedException` for a base method, it violates LSP.
- If a subtype constrains inputs or relaxes outputs beyond the base contract, it violates LSP.

### Signs of violation
- `isinstance()` or `type()` checks before calling methods.
- Subtypes that throw "unsupported" exceptions for inherited methods.
- Tests that must branch on concrete type to handle behavioral differences.
