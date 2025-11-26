# File Structure

Project and file layout MUST be consistent across repositories to reduce cognitive load.

## Script vs Module

- Standalone automation or entrypoints: **`.ps1` scripts**.
- Reusable functions: **modules** (`.psm1` + `.psd1`).

Scripts MUST NOT contain large numbers of unrelated functions.
Reusable logic MUST be moved into a module and imported.

## Recommended Layout

```text
MyProject/
├── src/
│   └── MyModule/
│       ├── MyModule.psd1
│       ├── MyModule.psm1
│       ├── Public/
│       │   └── Get-Thing.ps1
│       └── Private/
│           └── Convert-Thing.ps1
├── scripts/
│   └── Invoke-MyProject.ps1
└── tests/
    └── MyModule.Tests.ps1
```

## Script Requirements

Every `.ps1` script:

- MUST start with a **comment‑based help** block.
- MUST `Set-StrictMode -Version Latest`.
- SHOULD call `Set-PSDebug -Strict` in development scenarios (not production).
- MUST NOT define public reusable functions; those belong in modules.

## Module Requirements

Every module:

- MUST have a **manifest** (`.psd1`).
- MUST explicitly export only public functions.
- MUST keep public and private functions in separate folders (`Public/`, `Private/`).
- MUST NOT perform side effects at import time (no heavy initialization in module scope).
