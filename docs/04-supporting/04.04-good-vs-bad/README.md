# 17. Good vs Bad Examples

This chapter provides practical examples following the PowerShell Style Guide.
Every example includes a **Bad** version (anti-pattern) and a **Good** version (recommended pattern).

---

# 17.0 Formatting & Readability

## 17.0.1 Backtick Abuse

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

# 17.1 Naming Conventions

## 17.1.1 Non-descriptive variables

**Bad**
```powershell
$a = "John"
$b = "Doe"
$c = "$a $b"
```

**Good**
```powershell
$FirstName = "John"
$LastName  = "Doe"
$FullName  = "$FirstName $LastName"
```

## 17.1.2 Boolean naming

**Bad**
```powershell
$active = $true
```

**Good**
```powershell
$IsActive = $true
```

---

# 17.2 Aliases & Positional Parameters

## 17.2.1 Alias overuse

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

# 17.3 String Handling

## 17.3.1 Use single quotes for literals

**Bad**
```powershell
Write-Output "C:\Temp\Logs"
```

**Good**
```powershell
Write-Output 'C:\Temp\Logs'
```

## 17.3.2 Avoid unnecessary subexpressions

**Bad**
```powershell
"Count: $($items.Count)"
```

**Good**
```powershell
"Count: $itemsCount"
```

---

# 17.4 Error Handling

## 17.4.1 Avoid using `$?`

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

## 17.4.2 Catch specific exception types

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

# 17.5 Splatting

## 17.5.1 Avoid dynamic keys unless needed

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

## 17.5.2 Avoid uppercase/mixed-case parameter keys

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

# 17.6 Pipeline Usage

## 17.6.1 Avoid useless pipelines

**Bad**
```powershell
"Done" | Write-Output
```

**Good**
```powershell
Write-Output "Done"
```

## 17.6.2 Avoid manual loops when pipeline is better

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

# 17.7 Verbose, Warning, Debug

## 17.7.1 User-impacting warnings

**Bad**
```powershell
Write-Host "User does not exist"
```

**Good**
```powershell
Write-Warning "User does not exist: $UserName"
```

## 17.7.2 Debug for developer-only output

**Bad**
```powershell
Write-Host "Connecting to $server"
```

**Good**
```powershell
Write-Debug "Connecting to $server"
```

---

# 17.8 Advanced Functions

## 17.8.1 Use SupportsShouldProcess

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

## 17.8.2 Validate input early

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

# 17.9 Modules

## 17.9.1 Avoid executable code in module scope

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

## 17.9.2 Export only public commands

**Bad**
```powershell
Export-ModuleMember -Function *
```

**Good**
```powershell
Export-ModuleMember -Function Get-Thing, Set-Thing
```

---

# 17.10 Performance

## 17.10.1 Avoid expensive calls inside loops

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

## 17.10.2 Use built-in constants instead of counting large arrays

**Bad**
```powershell
(1..1MB).Count
```

**Good**
```powershell
1MB
```

---

# 17.11 Security

## 17.11.1 Do not print secrets

**Bad**
```powershell
Write-Host "Password: $Password"
```

**Good**
```powershell
Write-Verbose "Password received"
```

## 17.11.2 Avoid basic authentication if possible

**Bad**
```powershell
Invoke-RestMethod -Headers @{ Authorization = "Basic $Token" }
```

**Good**
```powershell
Invoke-RestMethod -Authentication OAuth -Token $OAuthToken
```

---

# 17.12 Testing

## 17.12.1 Test parameter validation explicitly

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

## 17.12.2 Only mock external dependencies

**Bad**
```powershell
Mock Get-Date { "fake" }
```

**Good**
```powershell
Mock Invoke-RestMethod { @{ Status = "OK" } }
```
