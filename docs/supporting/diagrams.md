# Diagrams

Diagrams MUST be stored as **text‑based sources** (ASCII or Mermaid) whenever possible.

## Mermaid Diagrams

Use Mermaid syntax for flows and relationships:

```mermaid
graph TD;
    A[Caller] --> B[Public Function];
    B --> C[Private Helper];
    C --> D[External System];
```

Source files SHOULD live under:

- `assets/mermaid/` for Mermaid definitions.
- `assets/ascii/` for ASCII art.
