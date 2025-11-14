# 11. Scripting Best Practices

Scripts MUST be predictable, robust, and easy to read.

## 11.1 Aliases

- MUST NOT use aliases in scripts or modules (`ls`, `gc`, `%`, `?`).
- Always use full cmdlet names (`Get-ChildItem`, `Get-Content`, `ForEach-Object`).

## 11.2 Positional Parameters

- MUST NOT use positional parameters in non‑trivial code.
- Always use named parameters for clarity.

## 11.3 Pipelines

- Use pipelines for object transformations.
- Avoid mixing pipeline logic with complex control flow; push logic into functions.
