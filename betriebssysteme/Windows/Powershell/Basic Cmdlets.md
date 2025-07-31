# powershell cmdlets because better than cmd.exe.

## syntax pattern
verb-noun structure:
- `Get-Content`: read file contents
- `Set-Location`: change directory  
- `New-Item`: create files/folders
- `Remove-Item`: delete files/folders

## discovery commands
```powershell
Get-Command                              # list all available cmdlets
Get-Command -CommandType Function        # functions only
Get-Command *Service*                    # cmdlets containing "Service"
Get-Help Get-Process                     # help for specific cmdlet
Get-Help Get-Process -Examples           # show usage examples
```

## aliases
```powershell
Get-Alias                               # list all aliases
Get-Alias ls                            # what ls maps to (Get-ChildItem)
```

common aliases:
- `dir` = `Get-ChildItem`
- `cd` = `Set-Location`
- `cat` = `Get-Content`
- `ls` = `Get-ChildItem`

## module management
```powershell
Find-Module -Name "pattern*"            # search online repositories
Install-Module -Name "ModuleName"       # install from gallery
Get-Module -ListAvailable               # show installed modules
Import-Module ModuleName                # load module into session
```

## basic operations
```powershell
Get-ChildItem                           # list directory contents
Set-Location C:\Path                    # change directory
Get-Content file.txt                    # read file
Out-File -FilePath file.txt -InputObject "data"    # write file
```

## notes
[!] cmdlets return objects, not just text like cmd.exe
[!] `Find-Module` and `Install-Module` require internet connection
[!] use `Get-Help` extensively - it's your best friend