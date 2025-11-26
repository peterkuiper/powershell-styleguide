# Formatting

Formatting rules are **strict** and MUST be followed.

## Indentation

- MUST use **4 spaces** per indentation level.
- Tabs are NOT allowed.

```powershell
if ($condition) {
    Invoke-Thing
}
```

## Spacing

- One space around operators: `=`, `-eq`, `+`, `-`, etc.
- One space after commas.
- One space between keywords and parentheses: `if ($x)` NOT `if($x)`.

## Braces

- Opening brace on the **same line**.
- Closing brace on its own line aligned with the start of the construct.

```powershell
function Get-Example {
    if ($true) {
        "Example"
    }
}
```

## Line Length

- Maximum line length: **120 characters**.
- When exceeding 120 characters, code MUST be wrapped using:
    - Pipeline line breaks
    - Parameter wrapping
    - Splatting

## Pipeline Formatting

Pipelines MUST be written one stage per line when longer than a simple expression.

```powershell
Get-Process |
    Where-Object { $_.CPU -gt 0 } |
    Sort-Object -Property CPU -Descending |
    Select-Object -First 10 -Property Name, CPU
```
