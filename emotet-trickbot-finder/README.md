## PowerShell script for finding and removing Emotet and Trickbot footholds

This [PowerShell script](https://raw.githubusercontent.com/huntresslabs/free-tools/master/emotet-trickbot-finder/emotet-trickbot-finder.ps1) is designed to identify (and optionally remove) common Emotet and Trickbot footholds (services, scheduled tasks, etc.). The script can be modified to include new footholds you may encounter.

_Many thanks to the Bytes Computer & Network Solutions team, this script is based on their work._

**The script is unsupported and is provided as-is.**

### Notes
* The script must be run with **administrative privileges**. 
* By default, the script will only display the services, tasks, and files it finds. There is no output if it finds nothing.
* The script uses `schtasks.exe` to get a list of scheduled tasks. If the Task Scheduler service is not running, `schtasks.exe` will diplay `ERROR: The network address is invalid.`

### Usage

```
C:\> powershell -executionpolicy bypass -f c:\users\user\downloads\emotet_trickbot_finder.ps1
[*] Looking for process by name: mssvca
[*] Process: mssvca.exe (mssvca) - c:\windows\mssvca.exe

Name       Used (GB)     Free (GB) Provider      Root                                CurrentLocation
----       ---------     --------- --------      ----                                ---------------
HKU                                Registry      HKEY_USERS
[*] Found registry value: mttvca
[*] Service: 12345678 (12345678) - c:\12345678.exe
[*] Service: yxyxzeea (NewService1) - c:\NewService1.bat
[*] Task: msnEtcs - c:\task2.bat
[*] Task: msntcs - c:\task1.bat
```

Run with the **remove** argument to have the script remove the services/tasks/files.

```
C:\> powershell -executionpolicy bypass -f c:\users\user\downloads\emotet_trickbot_finder.ps1 remove
[*] Looking for process by name: mssvca
[*] Process: mssvca.exe (mssvca) - c:\windows\mssvca.exe
VERBOSE: Performing operation "Stop-Process" on Target "mssvca (3320)".
[!] Removing c:\windows\mssvca.exe...
VERBOSE: Performing operation "Remove File" on Target "C:\windows\mssvca.exe".

Name       Used (GB)     Free (GB) Provider      Root                                 CurrentLocation
----       ---------     --------- --------      ----                                 ---------------
HKU                                Registry      HKEY_USERS
[*] Found registry value: mttvca
[!] Removing registry value: mttvca
VERBOSE: Performing operation "Remove Property" on Target "Item:
HKEY_USERS\S-1-5-18\software\Microsoft\Windows\CurrentVersion\Run Property: mttvca".
[*] Attempting to terminate process based on file path: 'c:\stsvc.exe'...
VERBOSE: Performing operation "Stop-Process" on Target "stsvc (892)".
[!] Removing c:\stsvc.exe...
VERBOSE: Performing operation "Remove File" on Target "C:\stsvc.exe".
[*] Service: 12345678 (12345678) - c:\12345678.exe
VERBOSE: Performing operation "Stop-Service" on Target "12345678 (12345678)".
[*] Attempting to terminate process based on file path: 'c:\12345678.exe'...
VERBOSE: Performing operation "Stop-Process" on Target "12345678 (3884)".
[!] Removing c:\12345678.exe...
VERBOSE: Performing operation "Remove File" on Target "C:\12345678.exe".
[!] Deleting service: 12345678
[SC] DeleteService SUCCESS
[*] Service: yxyxzeea (NewService1) - c:\NewService1.bat
VERBOSE: Performing operation "Stop-Service" on Target "NewService1 (yxyxzeea)".
[*] Attempting to terminate process based on file path: 'c:\NewService1.bat'...
[!] Removing c:\NewService1.bat...
VERBOSE: Performing operation "Remove File" on Target "C:\NewService1.bat".
[!] Deleting service: yxyxzeea
[SC] DeleteService SUCCESS
[*] Task: msnEtcs - c:\task2.bat
[*] Attempting to terminate process based on file path: 'c:\task2.bat'...
[!] Removing c:\task2.bat...
VERBOSE: Performing operation "Remove File" on Target "C:\task2.bat".
[!] Deleting scheduled task: msnEtcs
SUCCESS: The scheduled task "msnEtcs" was successfully deleted.
[*] Task: msntcs - c:\task1.bat
[*] Attempting to terminate process based on file path: 'c:\task1.bat'...
[!] Removing c:\task1.bat...
VERBOSE: Performing operation "Remove File" on Target "C:\task1.bat".
[!] Deleting scheduled task: msntcs
SUCCESS: The scheduled task "msntcs" was successfully deleted.
```
