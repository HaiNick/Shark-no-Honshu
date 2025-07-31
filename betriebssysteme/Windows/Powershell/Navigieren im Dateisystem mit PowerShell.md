# powershell filesystem navigation because windows file management.

## directory listing
```powershell
Get-ChildItem                           # list current directory (like dir/ls)
Get-ChildItem C:\Path\To\Directory      # list specific directory
Get-ChildItem -Recurse                  # recursive listing (all subdirs)
Get-ChildItem -Filter "*.txt"           # filter by file type
Get-ChildItem -Hidden                   # show hidden files
```

## navigation
```powershell
Set-Location C:\Path\To\Directory       # change directory (like cd)
Set-Location ..                         # go up one directory
Get-Location                            # show current directory (like pwd)
Push-Location C:\Temp                   # save current location and change
Pop-Location                            # return to saved location
```

## file/directory creation
```powershell
New-Item -Path "C:\Path\NewFile.txt" -ItemType File          # create file
New-Item -Path "C:\Path\NewFolder" -ItemType Directory      # create directory
New-Item -Path "C:\Path\Link.txt" -ItemType SymbolicLink -Target "C:\Path\Target.txt"    # create symlink
New-Item -Path "C:\Path\Hard.txt" -ItemType HardLink -Target "C:\Path\Target.txt"        # create hardlink
```

## file operations
```powershell
Copy-Item file.txt backup.txt          # copy file
Move-Item file.txt C:\NewLocation\     # move file
Remove-Item file.txt                   # delete file
Rename-Item oldname.txt newname.txt    # rename file
```

## useful aliases
- `dir` = `Get-ChildItem`
- `cd` = `Set-Location`
- `sl` = `Set-Location`
- `pwd` = `Get-Location`
- `copy` = `Copy-Item`
- `move` = `Move-Item`
- `del` = `Remove-Item`

## advanced patterns
```powershell
Get-ChildItem -Recurse -Include "*.log" | Remove-Item    # delete all .log files recursively
Get-ChildItem | Where-Object {$_.Length -gt 1MB}        # files larger than 1MB
Get-ChildItem | Sort-Object LastWriteTime -Descending   # sort by modification time
```

## notes
[!] `-Recurse` on large directories can be slow
[!] `Remove-Item` with `-Recurse` permanently deletes - no recycle bin
[!] symbolic links require admin privileges to create