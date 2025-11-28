# Documentation

Comments that contradict the code are worse than no comments. Always make a priority of keeping the comments up-to-date when the code changes!

Comments should be in English, and should be complete sentences. If the comment is short, the period at the end can be omitted.

Remember that comments should serve to your reasoning and decision-making, not attempt to explain what a command does. With the exception of regular expressions, well-written PowerShell can be pretty self-explanatory.

## Block Comments

Block comments generally apply to some or all of the code which follows them, and are indented to the same level as that code. Each line should start with a # and a single space.

## Inline comments

Comments on the same line as a statement can be distracting, but when they don't state the obvious, and particularly when you have several short lines of code which need explaining, they can be useful.

They should be separated from the code statement by at least two spaces, and ideally, they should line up with any other inline comments in the same block of code.

```PowerShell
$Options = @{
    Margin = 2          # The rendering box obscures a couple of pixels.
    Padding = 2         # We need space between the border and the text.
    FontSize = 24       # Keep this above 16 so it's readable in presentations.
}
```

## Comment‑Based Help

Every public function MUST have a comment‑based help block:

```powershell
function Get-Thing {
    <#
    .SYNOPSIS
        A brief description of the function or script.

    .DESCRIPTION
        A longer description.

    .PARAMETER FirstParameter
        Description of each of the parameters.
        Note:
        To make it easier to keep the comments synchronized with changes to the parameters,
        the preferred location for parameter documentation comments is not here,
        but within the param block, directly above each parameter.

    .PARAMETER SecondParameter
        Description of each of the parameters.

    .INPUTS
        Description of objects that can be piped to the script.

    .OUTPUTS
        Description of objects that are output by the script.

    .EXAMPLE
        Example of how to run the script.

    .LINK
        Links to further documentation.

    .NOTES
        Detail on what the script does, if this is needed.
    #>
    [CmdletBinding()]
    param(
        [Parameter(Mandatory)]
        [string]$Name
    )
}
```

### Document Each Parameter

Each parameter should be documented. To make it easier to keep the comments synchronized with changes to the parameters, the preferred location for parameter documentation comments is within the param block, directly above each parameter.

## README Files

Each project and module MUST have a `README.md` that:

- Explains purpose and scope.
- Documents prerequisites.
- Shows example usage.
