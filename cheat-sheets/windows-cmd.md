# windows command line operations because powershell isn't always available. example: file ops, process control, system info

## file and directory operations
```cmd
dir                            # list directory contents
dir /a                         # show hidden files too
dir /s                         # recursive listing
cd c:\windows                  # change directory
cd ..                          # go up one directory
md newdir                      # create directory
rd olddir                      # remove empty directory
rd /s /q olddir                # [!] force remove directory and contents

copy file.txt dest\            # copy file
xcopy source dest /e /i        # copy directory tree
move file.txt newname.txt      # rename/move file
del file.txt                   # delete file
del /f /q *.tmp                # force delete all tmp files
```

## file content and searching
```cmd
type file.txt                  # display file contents
more file.txt                  # page through file
find "text" file.txt           # search for text in file
find /i "text" file.txt        # case insensitive search
findstr "pattern" *.txt        # search pattern in multiple files
findstr /r "regex" file.txt    # regex search 
fc file1.txt file2.txt         # compare files
```

## process management
```cmd
tasklist                       # list running processes
tasklist /svc                  # show services per process
tasklist /m                    # show loaded modules
taskkill /pid 1234             # kill process by pid
taskkill /f /pid 1234          # force kill process
taskkill /im notepad.exe       # kill by image name
taskkill /f /im explorer.exe   # [!] kill explorer

# start processes
start notepad.exe              # start application
start /min calc.exe            # start minimized
start /b command               # start without new window
```

## service management
```cmd
sc query                       # list all services
sc query state= all            # show all service states
sc start service_name          # start service
sc stop service_name           # stop service
sc config service_name start= disabled  # disable service
net start                      # list running services
net start service_name         # start service (alt method)
net stop service_name          # stop service
```

## network operations
```cmd
ipconfig                       # show ip configuration
ipconfig /all                  # detailed network info
ipconfig /release              # release dhcp lease
ipconfig /renew                # renew dhcp lease
ipconfig /flushdns             # clear dns cache
ipconfig /registerdns          # register dns

ping 8.8.8.8                   # ping host
ping -t google.com             # continuous ping
tracert google.com             # trace route to host
nslookup google.com            # dns lookup
netstat -an                    # show network connections
netstat -b                     # show process per connection
arp -a                         # show arp table
```

## user and security
```cmd
whoami                         # current user
whoami /all                    # detailed user info
net user                       # list local users
net user username              # show user details
net user username password /add    # add new user
net localgroup administrators      # list admin group members
net localgroup administrators username /add  # add user to admins

runas /user:admin cmd          # run as different user
```

## system information
```cmd
systeminfo                     # detailed system info
hostname                       # computer hostname
ver                            # windows version
echo %computername%            # computer name
echo %username%                # current username
echo %userprofile%             # user profile path
set                            # show all environment variables
wmic os get version            # wmi os version
wmic process list brief        # wmi process list
```

## registry operations
```cmd
reg query HKLM\Software        # query registry key
reg add HKCU\Software\test     # add registry key
reg delete HKCU\Software\test  # delete registry key
reg export HKLM\Software backup.reg  # export registry
reg import backup.reg          # import registry
```

## scheduled tasks
```cmd
schtasks                       # list scheduled tasks
schtasks /query /tn taskname   # query specific task
schtasks /create /tn "mytask" /tr "c:\script.bat" /sc daily /st 10:00
schtasks /delete /tn "mytask"  # delete task
at 22:00 shutdown /s /t 0      # schedule shutdown (legacy)
```

## file attributes and permissions
```cmd
attrib +h file.txt             # set hidden attribute
attrib -r *.txt                # remove read-only from all txt
attrib +s +h folder            # set system and hidden
icacls file.txt                # show file permissions
icacls file.txt /grant user:f  # grant full control
icacls file.txt /deny user:w   # deny write access
takeown /f file.txt            # take ownership
```

## disk and file system
```cmd
chkdsk c:                      # check disk
chkdsk c: /f                   # fix errors
defrag c:                      # defragment drive
diskpart                      # disk partitioning tool
fsutil fsinfo drives          # list drives
fsutil volume diskfree c:     # check free space
compact /c /s:c:\folder        # compress folder
```

## environment and variables
```cmd
set PATH                       # show path variable
set PATH=%PATH%;c:\tools       # append to path
setx PATH "%PATH%;c:\tools" /m # set permanent path (system-wide)
set myvar=value                # set temporary variable
echo %myvar%                   # display variable
```

## batch scripting essentials
```cmd
# comments use rem or ::
rem this is a comment
:: this is also a comment

# conditional execution
if exist file.txt echo found
if not exist file.txt echo missing
if %errorlevel%==0 echo success

# loops
for %%i in (*.txt) do echo %%i
for /l %%i in (1,1,10) do echo %%i

# goto and labels
goto end
:end
echo done
```

## quick one-liners
```cmd
dir /s /b *.exe > executables.txt     # find all exe files
type file1.txt file2.txt > combined.txt   # combine files
echo text >> file.txt                 # append to file
date /t & time /t                     # show date and time
shutdown /s /t 3600                   # shutdown in 1 hour
tree /f                               # show directory tree with files
```

## danger zone [!]
```cmd
format c:                      # [!] format c drive - data loss
rd /s /q c:\                   # [!] delete everything on c:
del /f /s /q c:\*.*            # [!] delete all files on c:
shutdown /s /t 0               # immediate shutdown
taskkill /f /im winlogon.exe   # [!] crash system
```

## privilege escalation attempts
```cmd
# check for always install elevated
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer\AlwaysInstallElevated
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer\AlwaysInstallElevated

# check for unquoted service paths
wmic service get name,displayname,pathname,startmode |findstr /i "auto" |findstr /i /v "c:\windows\\" |findstr /i /v """

# check for modifiable services
sc query state= all | findstr "SERVICE_NAME:" >> services.txt
```

# TODO: add powershell execution, wmi commands, event log management, certificate operations