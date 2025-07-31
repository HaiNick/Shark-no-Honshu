# powershell piping and filtering because objects flow better than text.

## piping objects
powershell pipes objects (not just text) with properties and methods:
```powershell
Get-ChildItem | Sort-Object Length          # sort files by size
Get-Process | Where-Object CPU -gt 100      # processes using >100 CPU
Get-Service | Select-Object Name,Status     # show only name and status
```

## filtering with Where-Object
```powershell
Get-ChildItem | Where-Object Extension -eq ".txt"           # filter by file extension
Get-ChildItem | Where-Object Name -like "ship*"            # wildcard matching
Get-Process | Where-Object {$_.CPU -gt 50}                 # script block syntax
Get-EventLog System | Where-Object EntryType -eq "Error"   # system errors only
```

## comparison operators
```
-eq     # equals
-ne     # not equals  
-gt     # greater than
-ge     # greater than or equal
-lt     # less than
-le     # less than or equal
-like   # wildcard matching
-match  # regex matching  
-in     # contained in array
```

## selecting properties
```powershell
Get-Process | Select-Object Name,CPU,Memory     # specific properties
Get-ChildItem | Select-Object -First 10        # first 10 items
Get-Service | Select-Object -Last 5             # last 5 items
Get-Process | Select-Object -Unique Name        # unique names only
```

## sorting
```powershell
Get-ChildItem | Sort-Object Length                      # sort by size
Get-Process | Sort-Object CPU -Descending               # highest CPU first
Get-ChildItem | Sort-Object LastWriteTime -Descending   # newest files first
```

## grouping
```powershell
Get-Process | Group-Object ProcessName          # group by process name
Get-Service | Group-Object Status               # group by status
Get-ChildItem | Group-Object Extension          # group by file type
```

## practical examples
```powershell
# large files in current directory
Get-ChildItem | Where-Object Length -gt 1MB | Sort-Object Length -Descending

# running services starting with 'w'
Get-Service | Where-Object {$_.Status -eq "Running" -and $_.Name -like "W*"}

# top 10 CPU processes
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10 Name,CPU
```

## notes
[!] objects preserve type information through pipeline
[!] use `Get-Member` to see available properties: `Get-Process | Get-Member`
[!] script blocks `{$_.Property}` allow complex filtering logic