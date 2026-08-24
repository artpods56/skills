## Single Responsibility Principle (SRP)

> "A class should have only one reason to change." — Robert C. Martin

A module, class, or function should have responsibility over **one single part** of the functionality. All its services should be narrowly aligned with that responsibility. This is about **cohesion** — the strength of association between elements inside a module.

### Why it matters
- A class that handles business logic, persistence, and UI rendering has **multiple reasons to change**. Any change in the database schema, the UI framework, or the business rules forces edits in the same place.
- High cohesion means changes are isolated — you can modify one responsibility without worrying about the others.

### Guidelines
- **Prefer cohesive software entities.** Everything that does not strictly belong together should be separated.
- A `Circle` class should not contain `draw()`, `serialize()`, and geometry logic. Split geometry, drawing, and serialization into separate concerns.
- **One reason to change** — if you can list more than one actor/stakeholder who would request a change to a class, it likely violates SRP.

### Signs of violation
- A class has many `import` statements from unrelated domains.
- A module handles both data access and business rules.
- You feel compelled to comment sections of a class to separate "parts."
- Changes in one feature area force you to open a class that "does everything."

### Practical approach
- Group functionality by **actor** (who requests the change), not by theme.
- Use **facades** or **mediators** to coordinate between cohesive components.
- When in doubt, split — it's easier to combine small, focused components than to extract responsibilities from a monolith.
