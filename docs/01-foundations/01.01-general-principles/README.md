# 1. General Principles

This style guide defines **strict, mandatory rules** for PowerShell **7.4+** code.  
All examples and rules assume cross‑platform PowerShell Core, not Windows PowerShell 5.1.

## 1.1 Goals

- Maximize **readability** and **maintainability**.
- Minimize ambiguity and “personal style”.
- Make code easy to review, test, and automate.
- Enable future **linting** and **automated enforcement**.

## 1.2 Strictness

These rules are **normative**:

- “MUST” and “MUST NOT” are absolute.
- “SHOULD” is strongly recommended; deviations require justification in code review.
- “MAY” is optional and driven by context.

Code that does not follow this guide is considered **non‑compliant** and SHOULD be corrected.

## 1.3 Baseline Assumptions

All scripts and modules:

- MUST target **PowerShell 7.4 or later**.
- MUST run on **at least Windows and Linux** unless explicitly documented otherwise.
- MUST NOT rely on legacy, Windows‑only features without clearly documented justification.
- MUST be written in **English** for identifiers, comments, and documentation.

## 1.4 Core Principles

1. **Clarity over cleverness**  
   - Prefer explicit, readable code over compact or “smart” tricks.

2. **Objects over strings**  
   - Output SHOULD be strongly‑typed objects; formatted strings are for display only.

3. **Predictable behavior**  
   - Functions MUST behave consistently, with clearly defined inputs and outputs.

4. **Fail fast and loudly**  
   - Errors MUST be surfaced clearly and SHOULD NOT be silently swallowed.

5. **Single responsibility**  
   - Each function SHOULD do one thing well.

## 1.5 Example: Clear vs Clever

**Non‑compliant:**

```powershell
# Tries to be clever, hard to maintain
Get-Process | ? cpu | % n
```

**Compliant:**

```powershell
Get-Process |
    Where-Object { $_.CPU -gt 0 } |
    Select-Object -ExpandProperty Name
```
