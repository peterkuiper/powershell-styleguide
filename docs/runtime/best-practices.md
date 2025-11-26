# Best Practices

Scripts MUST be predictable, robust, and easy to read.

## Aliases

- MUST NOT use aliases in scripts or modules (`ls`, `gc`, `%`, `?`).
- Always use full cmdlet names (`Get-ChildItem`, `Get-Content`, `ForEach-Object`).

## Positional Parameters

- MUST NOT use positional parameters in non‑trivial code.
- Always use named parameters for clarity.

## Pipelines

- Use pipelines for object transformations.
- Avoid mixing pipeline logic with complex control flow; push logic into functions.
