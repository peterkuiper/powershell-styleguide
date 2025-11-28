# Formatting

Formatting rules are **strict** and MUST be followed.

## Indentation

- MUST use **4 spaces** per indentation level.
- Tabs are NOT allowed.

```powershell
if ($condition) {
    Invoke-Thing
}
```

Indenting more than 4-spaces is acceptable for continuation lines (when you're wrapping a line which was too long). In such cases you might indent more than one level, or even indent indent an odd number of spaces to line up with a method call or parameter block on the line before.

```powershell
function Test-Code {
    foreach ($base in 1,2,4,8,16) {
        foreach ($exponent in 1..10) {
            [System.Math]::Pow($base,
                               $exponent)
    }
}
```

## Spacing

- One space around operators: `=`, `-eq`, `+`, `-`, etc.
- One space after commas.
- One space between keywords and parentheses: `if ($x)` NOT `if($x)`.

One notable exception is when using colons to pass values to switch parameters:

```powershell
# Do not write:
$variable=Get-Content $FilePath -Wait:($ReadCount-gt0) -First($ReadCount*5)

# Instead write:
$variable = Get-Content -Path $FilePath -Wait:($ReadCount -gt 0) -TotalCount ($ReadCount * 5)
```

Another exception is when using [Unary Operators](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_operators#unary-operators):

```PowerShell
# Do not write:
$yesterdaysDate = (Get-Date).AddDays( - 1)

$i = 0
$i ++

# Instead write:
$yesterdaysDate = (Get-Date).AddDays(-1)

$i = 0
$i++

# Same principle should be applied when using a variable.
$yesterdaysDate = (Get-Date).AddDays(-$i)
```

## Braces

- Opening brace on the **same line**.
- Closing brace on its own line aligned with the start of the construct.

```powershell
function Get-Example {
    if ($true) {
        "True Example"
    } else {
        "False Example"
    }
}

# An exception case:
Get-ChildItem | Where-Object { $_.Length -gt 10mb }
```

## Line Length

- Maximum line length: **120 characters**.
- When exceeding 120 characters, code MUST be wrapped using:
    - Pipeline line breaks
    - Parameter wrapping
    - Splatting

## Trailing Spaces

Lines should NOT have trailing whitespace. Extra spaces result in future edits where the only change is a space being added or removed, making the analysis of the changes more difficult for no reason.

## Avoid Using Semicolons (;) as Line Terminators

PowerShell will not complain about extra semicolons, but they are unnecessary, and can get in the way when code is being edited or copy-pasted. They also result in extra do-nothing edits in source control when someone finally decides to delete them.

## Avoid backticks

Consider:

```PowerShell
Get-WmiObject -Class Win32_LogicalDisk `
              -Filter "DriveType=3" `
              -ComputerName SERVER2
```

In general, the community feels you should avoid using those backticks as "line continuation characters" when possible. They're hard to read, easy to miss, and easy to mistype. Also, if you add an extra whitespace after the backtick in the above example, then the command won't work. The resulting error is hard to correlate to the actual problem, making debugging the issue harder.

## Pipeline Formatting

Pipelines MUST be written one stage per line when longer than a simple expression.

```powershell
Get-Process |
    Where-Object { $_.CPU -gt 0 } |
    Sort-Object -Property CPU -Descending |
    Select-Object -First 10 -Property Name, CPU
```
