# Error Handling

Error handling MUST be explicit and predictable.

## Terminating vs Non‑Terminating

- All critical operations MUST use `-ErrorAction Stop`.
- `try`/`catch` MUST be used for recoverable errors.

```powershell
try {
    $item = Get-Item -Path $Path -ErrorAction Stop
} catch {
    throw "Failed to retrieve item at path '$Path'."
}
```

## Avoid Swallowing Errors

`catch` blocks MUST NOT be empty and MUST NOT silently ignore failures.

**Non‑compliant:**

```powershell
try {
    Do-Something
} catch {
    # ignore
}
```

**Compliant:**

```powershell
catch {
    Write-Error -ErrorRecord $_
    throw
}
```
