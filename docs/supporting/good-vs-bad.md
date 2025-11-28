# Good vs Bad Examples

This chapter provides practical examples following the PowerShell Style Guide.
Every example includes a **Bad** version (anti-pattern) and a **Good** version (recommended pattern).

---

## Formatting & Readability

### Backtick Abuse

**Bad**

```powershell
Get-ADUser -Filter * `
-Properties Name,EmailAddress,Department,Title,Manager `
-SearchBase "OU=Users,DC=contoso,DC=com"
```

**Good**

```powershell
Get-ADUser -Filter *
           -Properties Name, EmailAddress, Department, Title, Manager
           -SearchBase "OU=Users,DC=contoso,DC=com"
```

**Better (Splatting)**

```powershell
$params = @{
    Filter     = "*"
    Properties = "Name", "EmailAddress", "Department", "Title", "Manager"
    SearchBase = "OU=Users,DC=contoso,DC=com"
}

Get-ADUser @params
```

---

## Naming Conventions

### Non-descriptive variables

**Bad**

```powershell
$a = "John"
$b = "Doe"
$c = "$a $b"
```

**Good**

```powershell
$firstName = "John"
$lastName  = "Doe"
$fullName  = "$firstName $lastName"
```

### Boolean naming

**Bad**

```powershell
$active = $true
```

**Good**

```powershell
$isActive = $true
```

---

## Aliases & Positional Parameters

### Alias overuse

**Bad**

```powershell
gci env: | ? name -match "PATH" | % value
```

**Good**

```powershell
Get-ChildItem env: |
    Where-Object Name -match "PATH" |
    Select-Object -ExpandProperty Value
```

---

# String Handling

##  Use single quotes for literals

**Bad**

```powershell
Write-Output "C:\Temp\Logs"
```

**Good**

```powershell
Write-Output 'C:\Temp\Logs'
```

## Avoid unnecessary subexpressions

**Bad**

```powershell
"Count: $($items.Count)"
```

**Good**

```powershell
"Count: $itemsCount"
```

---

# Error Handling

## Avoid using `$?`

**Bad**

```powershell
Get-Content -Path $File
if (!$?) {
    Write-Error "Failed"
}
```

**Good**

```powershell
try {
    Get-Content -Path $File -ErrorAction Stop
} catch {
    Write-Error "Unable to read file: $File"
}
```

## Catch specific exception types

**Bad**

```powershell
try { 1/0 } catch { Write-Error "Fail" }
```

**Good**

```powershell
try { 1/0 }
catch [System.DivideByZeroException] {
    Write-Error "Cannot divide by zero."
}
```

---

# Splatting

## Avoid dynamic keys unless needed

**Bad**

```powershell
$key = "Path"
$params = @{ $key = "C:\Temp" }
Copy-Item @params
```

**Good**

```powershell
$params = @{
    Path = "C:\Temp"
}
Copy-Item @params
```

## Avoid uppercase/mixed-case parameter keys

**Bad**

```powershell
$params = @{ PATH = "C:\Temp" }
Copy-Item @params
```

**Good**

```powershell
$params = @{ Path = "C:\Temp" }
Copy-Item @params
```

---

# Pipeline Usage

## Avoid useless pipelines

**Bad**

```powershell
"Done" | Write-Output
```

**Good**

```powershell
Write-Output "Done"
```

## Avoid manual loops when pipeline is better

**Bad**

```powershell
$services = Get-Service
foreach ($s in $services) {
    if ($s.Status -eq 'Running') { $s.Name }
}
```

**Good**

```powershell
Get-Service |
    Where-Object Status -eq 'Running' |
    Select-Object -ExpandProperty Name
```

---

# Verbose, Warning, Debug

## User-impacting warnings

**Bad**

```powershell
Write-Host "User does not exist"
```

**Good**

```powershell
Write-Warning "User does not exist: $UserName"
```

## Debug for developer-only output

**Bad**

```powershell
Write-Host "Connecting to $server"
```

**Good**

```powershell
Write-Debug "Connecting to $server"
```

---

# Advanced Functions

## Use SupportsShouldProcess

**Bad**

```powershell
function Remove-Thing {
    param([string]$Path)
    Remove-Item -Path $Path -Recurse -Force
}
```

**Good**

```powershell
function Remove-Thing {
    [CmdletBinding(SupportsShouldProcess)]
    param([string]$Path)

    if ($PSCmdlet.ShouldProcess($Path, "Delete")) {
        Remove-Item -Path $Path -Recurse -Force
    }
}
```

## Validate input early

**Bad**

```powershell
param($Path)
Get-ChildItem -Path $Path
```

**Good**

```powershell
param(
    [Parameter(Mandatory)]
    [ValidateNotNullOrEmpty()]
    [ValidateScript({ Test-Path $_ })]
    [string]$Path
)
Get-ChildItem -Path $Path
```

---

# Modules

## Avoid executable code in module scope

**Bad**

```powershell
# MyModule.psm1
Write-Host "Loaded!"
```

**Good**

```powershell
# MyModule.psm1
# (No executable code)
```

## Export only public commands

**Bad**

```powershell
Export-ModuleMember -Function *
```

**Good**

```powershell
Export-ModuleMember -Function Get-Thing, Set-Thing
```

---

# Performance

## Avoid expensive calls inside loops

**Bad**

```powershell
foreach ($user in $users) {
    Get-ADUser -Identity $user
}
```

**Good**

```powershell
Get-ADUser -Identity $users
```

## Use built-in constants instead of counting large arrays

**Bad**

```powershell
(1..1MB).Count
```

**Good**

```powershell
1MB
```

---

# Security

## Do not print secrets

**Bad**

```powershell
Write-Host "Password: $Password"
```

**Good**

```powershell
Write-Verbose "Password received"
```

## Avoid basic authentication if possible

**Bad**

```powershell
Invoke-RestMethod -Headers @{ Authorization = "Basic $Token" }
```

**Good**

```powershell
Invoke-RestMethod -Authentication OAuth -Token $OAuthToken
```

---

# Testing

## Test parameter validation explicitly

**Bad**

```powershell
It "handles failures" {
    Get-Thing | Should -Throw
}
```

**Good**

```powershell
It "throws on empty name" {
    { Get-Thing -Name "" } |
        Should -Throw -ErrorId "ParameterBindingValidationException"
}
```

## Only mock external dependencies

**Bad**

```powershell
Mock Get-Date { "fake" }
```

**Good**

```powershell
Mock Invoke-RestMethod { @{ Status = "OK" } }
```
