# Azure CLI Usage

## When Azure CLI Is Acceptable

-   A required Azure capability is only available in Azure CLI.
-   Azure PowerShell has a known limitation or bug preventing correct
    usage.
-   Cross‑platform parity is required for scripts that run on Windows,
    macOS, and Linux.

## Required Usage Practices

### Invoke Azure CLI Commands Directly

Always call `az` directly without wrapping it in aliases or helper
functions.

### Validate `LASTEXITCODE` After Every Call

Azure CLI communicates success and failure through exit codes. Always
check `LASTEXITCODE` immediately after each invocation.

``` powershell
az group list --output json

if ($LASTEXITCODE -ne 0) {
    throw "Azure CLI command failed with exit code $LASTEXITCODE"
}
```

### Parse Output Explicitly

Request structured output and convert it for use in PowerShell.

``` powershell
$groups = az group list --output json | ConvertFrom-Json

if ($LASTEXITCODE -ne 0) {
    throw "Azure CLI command failed with exit code $LASTEXITCODE"
}
```

### Handle Authentication Explicitly

Validate authentication before making calls. For non‑interactive
scripts, ensure `az login`, managed identity, or workload identity is
configured.

### Avoid Silent Failures

Do not suppress Azure CLI errors unless absolutely necessary.

## Preferred Alternatives

Use Azure CLI only when Azure PowerShell, ARM/Bicep, or Graph/Entra
modules cannot meet the requirement.
