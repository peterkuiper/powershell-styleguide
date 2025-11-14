# 7. Documentation

All public functions and scripts MUST be documented.

## 7.1 Comment‑Based Help

Every public function MUST have a comment‑based help block:

```powershell
function Get-Thing {
    <#
    .SYNOPSIS
    Gets a thing by name.

    .DESCRIPTION
    Retrieves a thing from the data source.

    .PARAMETER Name
    The name of the thing.

    .EXAMPLE
    Get-Thing -Name 'Example'
    #>
    [CmdletBinding()]
    param(
        [Parameter(Mandatory)]
        [string]$Name
    )
}
```

## 7.2 README Files

Each project and module MUST have a `README.md` that:

- Explains purpose and scope.
- Documents prerequisites.
- Shows example usage.
