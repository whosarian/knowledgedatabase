# Powershell

!!! tip "Common Operations"
  * `eq`: equal
  * `ne`: not equal
  * `gt`: greater than
  * `ge`: greater or equal
  * `lt`: less than
  * `le`: less or equal
  * `match`: for regex
  * `like`

## What if

```ps1
... -WhatIf
```
You can use this parameter to check what would happen, if a command is executed, without actually executing it.

## Formatting

*Example 1: Set permissions on specified files*

```ps1
Get-Acl -Path C:\\Windows\\*.exe | Format-Table -Property pschildname , AccessToString -Wrap
```

This command includes:
  - `Format-Table`: Set table format
  - `-Property`: Define wanted information
  - `-Wrap`: Generates linebreak and blocks the cutting of lines

*Example 2: Show the last 10 event of the System-Log as table*

```ps1
 Get-WinEvent -LogName system -MaxEvents 10 | Format-Table -Property TimeCreated , Message -Wrap
```

*Example 3: Display all aliases as table with names and definitions*

```ps1
Get-WinEvent -LogName system -MaxEvents 10 | Format-List -Property TimeCreated , Message
```
!!! danger "Only the following may be formatted:"
  - Display on screen
  - Print
  - Redirect to a text file

Steps such as subsequent sorting, filtering, redirection to a `.csv` file, etc. are not possbile because format commands send graphical control instruction. The object structure is resolved.

## Determine commands properties and methods

`Get-Member` is a reference tool to see what a command can do.

```ps1
... | Get-Member
```

## Sorting

```ps1
... | Sort-Object
```

*Example 1: Sort volumes descending based on size*

```ps1
Get-Volume | Sort-Object -Property Size -Descending
```

## Export of data to files

```ps1
... | Out-File -FilePath
```

*Example: Export text file with name and activation status of firewall profiles*

```ps1
Get-NetFirewallProfile | Format-Table -Property Name, Enabled | Out-File -FilePath C:\\Daten\\Firewallprofile.txt
```

## Group data

*Example 1: Display files from home folder, sorted by extension and in descending order, to show how many files of each type are present*

```ps1
Get-ChildItem -Path C:\\Windows\\ -File | Group-Object -Property Extension -NoElement | Sort-Object -Property Count -Descending
```

## Filter data with `Select-Object`

Don't confuse this with `Format`. `Select-Object`s task is to determine what remains in the pipeline for further processing.

*Example 1: Create a `.csv`-file with selected BIOS-data*

```ps1
Get-CimInstance -ClassName Win32_BIOS | Select-Object -Property Manufacturer, Version, SerialNumber| Export-Csv -Path C:\\Daten\\bios.csv
```

*Example 2: Create a text file with all DNS names of the machines that have the operating system Windows Server 2019*

```ps1
Get-ADComputer -Filter {operatingsystem -like "*2019*"} | Select-Object -ExpandProperty dnshostname | Out-File -FilePath C:\\Windows\\server2019.txt
``` 

## Filter data with `Where-Object`

`Where-Object` is the only command in PS, thats able to evaluate conditions.

*Example 1: Show all processes that are using more than 50 MB RAM*

```ps1
Get-Process | Where-Object -FilterScript {$_.WorkingSet -gt 50mb}
```

This Command includes:
  - `Where-Object`
  - `{...}`: Script, that includes a condition, that must be given
  - `$_`: Placeholder for the object that is send to `Where-Object`
  - `.`: Binds the current object to the attribute being queried in order to determine a numerical value
  - `-gt`: "greater than" (>)

*Example 2; Display all processes that are using more than 50 MB RAM and more than 100 seconds CPU time*

```ps1
Get-Process | Where-Object -FilterScript {$_.WorkingSet -gt 50mb -and $_.CPU -gt 100}
```

## Modules

Modules are tools that mainly focus on one topic. A Module is also a script, so the execution must be allowed. They are loaded automatically if a command from the module is executed.

*Example 1: Display the modules of the current session*

```ps1
Get-Module
```

*Example 2: Display available modules*
```ps1
Get-Module -ListAvailable
```

*Example 3: Shows the directory from which modules are loaded*
```ps1
(Get-ChildItem -Path Env:\\PSModulePath).value -split ";"
```

!!! danger "PS 5 and PS 7 are not always able to use everything. This must be taken into account when choosing the storage location for the module.

*Example 4: Create new module as tool collection*

```ps1
function Get-DCFunction {
    Invoke-Command -ComputerName lon-dc1 -ScriptBlock {Get-Service -Name ntds, dns}
}

function Get-DisabledAccount {
    Search-ADAccount -AccountDisabled | Format-Table -Property Samaccountname
}
```

This uses the Powershell ISE. The file is saved in a new directory with the file extension `.psm1` under `C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\Modules`.  
The module name must match the name of the folder it is located in. The defined functions can then be easily called in Powershell.

## Remote administration

There are 3 ways to access remote systems. Server are listening on default. Clients must be opened up.

```ps1
Enable-PSRemoting
```

!!! tip "Parameters and commands"

1. `-ComputerName`: Access a specific system
2. `Invoke-Command`
3. `Enter-PSSession`: Enter a session on remote
3. `New-PSSession`: Create a session on remote

!!! warning "The execution policy on the local system has to allow execution. On remote it doesnt matter."

## Execution policies

Determine whether scripts are allowed to run, or whether they require a digital signature.  

!!! tip "Initial settings: Clients - Restricted; Servers - Remote Signed"
