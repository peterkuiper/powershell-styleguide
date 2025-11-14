# 9. Error Handling

Error handling MUST be explicit and predictable.

## 9.1 Terminating vs Non‑Terminating

- All critical operations MUST use `-ErrorAction Stop`.
- `try`/`catch` MUST be used for recoverable errors.

```powershell
try {
    $item = Get-Item -Path $Path -ErrorAction Stop
} catch {
    throw "Failed to retrieve item at path '$Path'."
}
```

## 9.2 Avoid Swallowing Errors

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
