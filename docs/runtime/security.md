# Security

Security MUST be considered in all scripts and modules.

## Credentials

- MUST NOT hard‑code credentials or secrets.
- MUST use `Get-Credential` or secret management solutions.
- MUST NOT log secrets or tokens.

## Input Validation

- Parameters that represent paths, IDs, or critical values MUST be validated.
- Use `[ValidateNotNullOrEmpty()]`, `[ValidateSet()]`, or custom validation where necessary.
