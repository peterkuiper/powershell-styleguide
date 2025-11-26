# Functions

Functions are the primary unit of reusable logic. All non‑trivial behavior MUST be implemented as **advanced functions**.

## Advanced Functions

All public functions MUST:

- Use `[CmdletBinding()]`.
- Declare a `param()` block.
- Support `-Verbose` and `-ErrorAction` semantics.

```powershell
function Get-Example {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory)]
        [string]$Name
    )

    Write-Verbose "Getting example for '$Name'"

    [PSCustomObject]@{
        Name   = $Name
        Status = "Example"
    }
}
```

## Parameters and Validation

Parameters MUST:

- Use appropriate .NET types (`[string]`, `[int]`, `[datetime]`, etc.).
- Use `[ValidateNotNullOrEmpty()]` where empty input is invalid.
- Use `[ValidateSet()]` for small enumeration‑like inputs.

## Pipeline Lifecycle

When accepting pipeline input:

- Use `begin`, `process`, and `end` blocks.
- Keep initialization in `begin`, per‑item work in `process`, and aggregation in `end`.

```powershell
function Get-ProcessedItem {
    [CmdletBinding()]
    param(
        [Parameter(ValueFromPipeline)]
        [string]$Name
    )

    begin {
        $count = 0
    }
    process {
        $count++
        [PSCustomObject]@{
            Name   = $Name
            Status = "Processed"
        }
    }
    end {
        Write-Verbose "Processed $count items."
    }
}
```
