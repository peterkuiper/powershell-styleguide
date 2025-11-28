# Naming Conventions

Naming MUST be predictable and follow strict patterns.

In general, prefer the use of full explicit names for commands and parameters rather than aliases or short forms. There are tools like [PSScriptAnalyzer](https://github.com/PowerShell/PSScriptAnalyzer)'s `Invoke-Formatter` and scripts like [Expand-Alias](https://github.com/PoshCode/ModuleBuilder/blob/master/PotentialContribution/ResolveAlias.psm1) for fixing many, but not all of these issues.

## Cmdlets and Functions

- MUST use `Verb-Noun` format.
- Verb MUST be from the **approved PowerShell verb list**.
- Noun MUST be **singular** (`Get-User`, not `Get-Users`), unless the function is conceptually plural (e.g. `Get-History`).

**Non‑compliant:**

```powershell
function doStuff { }
function GetUsers { }
```

**Compliant:**

```powershell
function Get-User { }
function Get-DirectoryItem { }
```

## Variables

- MUST use **camelCase** (`$userName`, `$logPath`).
- MUST be descriptive; single letters are only allowed for:
    - Loop counters: `$i`, `$j`
    - Very small scopes (e.g. short script blocks)

**Non‑compliant:**

```powershell
$x = Get-Content $p
```

**Compliant:**

```powershell
$logPath   = $Path
$logLines  = Get-Content -Path $logPath
```

## Parameters

- MUST use **PascalCase** (`-UserName`, `-LogPath`).
- MUST use common names where possible: `-Path`, `-Name`, `-Id`, `-Credential`.
- MUST avoid abbreviations unless widely recognized (`Id`, `Url`, `Api`).

## Files and Modules

- Module folders MUST match module name exactly (case‑insensitive).
- Script names MUST be verbs or short phrases describing their purpose:

```text
Invoke-Build.ps1
Publish-ModuleArtifacts.ps1
Initialize-DeveloperEnvironment.ps1
```
