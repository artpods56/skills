---
name: solid-review
description: Perform deep, evidence-based SOLID design reviews of code, repositories, pull-request diffs, patches, and changed files. Use this skill whenever the user asks for a SOLID review, architecture-quality review, design-principle audit, coupling/cohesion review, subtype-contract review, interface review, dependency-direction review, or a linter-like report of structural code-quality violations. Report proven SRP, OCP, LSP, ISP, and DIP violations without suggesting fixes.
compatibility: Requires read access to the reviewed code and, for diff reviews, the relevant patch or Git history.
---

# SOLID Review

Produce a code-quality report that behaves like a careful structural linter. Find places where the reviewed code violates one or more SOLID principles, prove each finding from code evidence, and stop there. Do not redesign the code or suggest fixes.

## Normative references

Read all five bundled references before reviewing code:

- [Single Responsibility Principle](references/solid/srp.md)
- [Open/Closed Principle](references/solid/ocp.md)
- [Liskov Substitution Principle](references/solid/lsp.md)
- [Interface Segregation Principle](references/solid/isp.md)
- [Dependency Inversion Principle](references/solid/dip.md)

Treat these references as the review standard. Do not import unrelated project rules, generic style preferences, Clean Code heuristics, framework conventions, or personal design rules into the verdict.

## Review contract

- Review only the scope requested by the user.
- In a diff review, report violations introduced or materially worsened by the diff. Read surrounding code, callers, implementors, tests, and configuration when needed to prove the finding, but do not report unrelated pre-existing violations.
- In a repository or directory review, inspect the relationships needed to assess the selected scope rather than reviewing isolated files in a vacuum.
- Report only violations supported by concrete evidence. Omit speculative, preference-based, and low-confidence concerns.
- Do not suggest fixes, alternatives, refactorings, abstractions, patterns, or next steps.
- Do not praise compliant code. A clean report may simply say that no supported violations were found.

## Deep review workflow

### 1. Establish scope

Identify the review mode and exact boundary:

- **Diff:** determine the base and head revisions or read the supplied patch.
- **Files/directories:** enumerate the requested files and their relevant collaborators.
- **Repository:** identify composition roots, domain or policy modules, infrastructure boundaries, public interfaces, and subtype families.

Record exclusions and unavailable evidence. Do not silently expand the review to unrelated repositories or generated/vendor code.

### 2. Build a structural map

Before judging individual lines, trace:

- modules, classes, functions, and their responsibilities;
- callers and callees across the requested boundary;
- interfaces, protocols, abstract classes, implementations, and clients;
- inheritance and substitution relationships;
- policy code, infrastructure details, and composition roots;
- repeated extension points such as type switches or coordinated edits.

Use searches and targeted reads liberally. A structural violation normally exists in a relationship, not in one line viewed alone.

### 3. Run five independent principle passes

Keep the passes independent so one suspected violation does not become evidence for another.

#### SRP pass

Identify distinct actors or reasons to change. Prove that one unit owns unrelated responsibilities or mixes change drivers. Size, method count, or many imports are signals to investigate, not violations by themselves.

#### OCP pass

Identify a real axis of variation and show that adding an expected type or behavior requires coordinated modification of stable code. A single conditional, enum, or switch is not enough without evidence that the variation is genuine and repeated or explicitly part of the domain.

#### LSP pass

Compare each subtype or implementation against the caller-visible contract of its base type. Check accepted inputs, returned guarantees, exceptions, side effects, invariants, and unsupported operations. Prove how substitution changes observable behavior.

#### ISP pass

Map interface members to actual clients and implementors. Show that a client or implementation is forced to depend on operations it does not need, cannot support, or must fake. Interface size alone is not sufficient evidence.

#### DIP pass

Trace dependency direction from high-level policy to low-level details. Show where policy directly owns or imports replaceable infrastructure, or where an abstraction is owned by the detail rather than shaped by the consuming policy. Concrete construction in a composition root is expected and is not a violation.

### 4. Challenge every candidate

For each candidate finding, actively search for counter-evidence:

- Is the apparently mixed responsibility actually one cohesive actor or policy?
- Is the alleged extension axis real, or merely hypothetical?
- Does the subtype contract permit the observed difference?
- Do all clients genuinely need the complete interface?
- Is concrete construction occurring at an intentional composition boundary?
- Was the issue introduced or worsened by the reviewed diff?

Discard the finding when a reasonable contract-consistent explanation remains or the necessary relationships cannot be inspected.

### 5. Consolidate findings

Merge duplicate evidence chains, but keep separate findings when the same code independently violates different principles. Choose one primary location and list only the related locations needed to prove the relationship.

## Severity and confidence

Use severity to describe demonstrated impact:

- **High:** breaks a substitutability contract, crosses a major architectural boundary, or causes broad mandatory change ripple.
- **Medium:** creates concrete coupling or mixed ownership that makes an ordinary change affect unrelated units.
- **Low:** creates a localized but still demonstrated SOLID violation.

Use only **high** or **medium** confidence findings:

- **High confidence:** the violation and its impact are directly demonstrated by definitions, callers, implementations, tests, or the diff.
- **Medium confidence:** the structural evidence is strong, but some runtime or historical evidence is unavailable.

Omit low-confidence candidates instead of filling the report with caveats.

## Report format

Use this structure exactly:

```markdown
# SOLID Review

## Scope
- Mode: diff | files | directory | repository
- Reviewed: <revision range, patch, or paths>
- Excluded: <generated/vendor/unavailable areas, or "None">

## Summary
- Findings: <total>
- By principle: SRP <n>, OCP <n>, LSP <n>, ISP <n>, DIP <n>
- By severity: High <n>, Medium <n>, Low <n>

## Findings

### [SOLID-<PRINCIPLE>-NNN] <short factual title>
- Severity: High | Medium | Low
- Confidence: High | Medium
- Location: `path/to/file.ext:line`
- Related locations: `path:line`, `path:line` | None
- Change attribution: Introduced | Worsened | Not applicable
- Principle: <full principle name>
- Evidence: <specific definitions, calls, branches, contracts, or dependencies>
- Violation: <why that evidence contradicts the normative principle>
- Impact: <demonstrated contract failure, coupling, or change ripple>

## Coverage and limitations
- <what relationships were inspected and what material evidence was unavailable>
```

Order findings by severity, then by source location. Number findings independently per principle, such as `SOLID-SRP-001` and `SOLID-DIP-001`.

If no supported violations are found, retain **Scope**, **Summary**, and **Coverage and limitations**, and replace **Findings** with:

```markdown
## Findings

No supported SOLID violations were found in the reviewed scope.
```

## Reporting discipline

- Make every finding independently verifiable from the cited code.
- Describe observed structure and behavior, not the reviewer’s preferred design.
- Avoid vague labels such as “poor design,” “not scalable,” or “too complex.”
- Do not turn code smells into SOLID violations without proving the relevant actor, variation, contract, client dependency, or dependency direction.
- Do not include a recommendations, remediation, suggested fix, or example patch section.
