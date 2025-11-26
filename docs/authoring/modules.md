# Modules

Modules encapsulate reusable logic. All shared production code MUST live in modules.

## Public vs Private

- Public functions MUST live in `Public/`.
- Private helpers MUST live in `Private/`.
- Only public functions MUST be exported from the module manifest.

```text
MyModule/
├── MyModule.psd1
├── MyModule.psm1
├── Public/
│   └── Get-Thing.ps1
└── Private/
    └── Convert-Thing.ps1
```

## Manifest

The module manifest (`.psd1`) MUST:

- Declare `RootModule`.
- Declare `FunctionsToExport` explicitly.
- Use semantic versioning.

## Side Effects

- Modules MUST NOT perform heavy work at import time.
- Initialization logic SHOULD be behind an explicit function (e.g. `Initialize-MyModule`).
