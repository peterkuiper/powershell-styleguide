# 12. Security

Security MUST be considered in all scripts and modules.

## 12.1 Credentials

- MUST NOT hard‑code credentials or secrets.
- MUST use `Get-Credential` or secret management solutions.
- MUST NOT log secrets or tokens.

## 12.2 Input Validation

- Parameters that represent paths, IDs, or critical values MUST be validated.
- Use `[ValidateNotNullOrEmpty()]`, `[ValidateSet()]`, or custom validation where necessary.
