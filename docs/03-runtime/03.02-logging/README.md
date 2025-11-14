# 10. Logging & Output

Diagnostic information MUST use the appropriate streams.

## 10.1 Streams

- Use `Write-Verbose` for optional diagnostic details.
- Use `Write-Debug` for low‑level troubleshooting.
- Use `Write-Information` for structured, user‑visible events.
- Use `Write-Error` for errors.

## 10.2 No Write-Host in Libraries

- `Write-Host` MUST NOT be used in reusable modules.
- It MAY be used in interactive scripts and tools, but NEVER for core logic or returned data.
