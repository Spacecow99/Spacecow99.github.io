
- return [$HOME](https://spacecow99.github.io/)

# NPCAP

## npcapwatchdog Scheduled Task Unquoted Path Interception

The `npcapwatchdog` scheduled task is set to execute `C:\Program Files\Npcap\CheckStatus.bat` at startup. As the path to execute contains a space yet is not quoted, there is a path interception at `C:\Program.exe` that will execute as SYSTEM when the task is run. While creating a file at `C:\Program.exe` requires Administrator priviledges, it allows us a path to escalate to SYSTEM.

<img src="https://Spacecow99.github.io/path-interception/npcap/npcap_path_interception.PNG" width="604" height="190" />

### Steps to Reproduce

1. Copy `C:\Windows\System32\calc.exe` to `C:\`.
2. Rename `calc.exe` to `Program.exe`.
3. Reboot or run the `npcapwatchdog` scheduled task.

<img src="https://Spacecow99.github.io/path-interception/npcap/npcap_scheduled_task.PNG" width="487" height="38" />
