# 3. Naming Conventions

Naming MUST be predictable and follow strict patterns.

## 3.1 Cmdlets and Functions

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

## 3.2 Variables

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

## 3.3 Parameters

- MUST use **PascalCase** (`-UserName`, `-LogPath`).
- MUST use common names where possible: `-Path`, `-Name`, `-Id`, `-Credential`.
- MUST avoid abbreviations unless widely recognized (`Id`, `Url`, `Api`).

## 3.4 Files and Modules

- Module folders MUST match module name exactly (case‑insensitive).
- Script names MUST be verbs or short phrases describing their purpose:

```text
Invoke-Build.ps1
Publish-ModuleArtifacts.ps1
Initialize-DeveloperEnvironment.ps1
```
