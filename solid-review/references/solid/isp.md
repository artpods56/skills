## Interface Segregation Principle (ISP)

> "Clients should not be forced to depend on methods that they do not use." — Robert C. Martin

Prefer **many small, client-specific interfaces** over one large, general-purpose interface. Fat interfaces couple clients to methods they don't need, making changes ripple unnecessarily.

### Why it matters
- When an interface has methods that only some clients use, a change to those methods forces recompilation/redeployment of **all** clients — even those that don't use the changed methods.
- Fat interfaces hide the real dependencies between components.

### The DrawStrategy example
Instead of one `DrawStrategy` interface with `draw(Circle)` and `draw(Square)`:
- Create `DrawCircleStrategy` with only `draw(Circle)`.
- Create `DrawSquareStrategy` with only `draw(Square)`.
- Each shape only depends on **its own** drawing strategy — no unnecessary coupling.

### Guidelines
- **Make sure interfaces don't induce unnecessary dependencies.**
- Keep interfaces focused on a single client or use case.
- If you find "empty" implementations of interface methods, the interface is too wide.
- In generic/template programming, require only the **minimum concept** needed.

### Signs of violation
- "Fat" interfaces with many methods where most implementors leave some empty or throw "unsupported."
- A change to one method on an interface forces changes in many unrelated implementations.
- Clients import or depend on large interfaces but only use one or two methods.
