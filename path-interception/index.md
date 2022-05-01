- return [$HOME](https://spacecow99.github.io/)

# Path Interception

| Application | Version | Binary Name | Binary Location |
| -- | -- | -- | -- |
| | | | |

## Git-GUI

### cygpath.com Path Interception

The `wish.exe` executable that comes packaged with the Windows Git GUI client is susceptible to a path interception where an attacker can take advantage of a user writable path folder to have a non-existent executable with the filename `cygpath.com` executed upon application startup. The `wish.exe` binary is looking to execute the legitimate `C:\Program Files (x86)\Git\usr\bin\cygpath.exe` but is being run with the command line `cygpath --windir`, causing it to begin a path search for the binary. Using the `.com` file extension ensures that the malicious binary will be loaded before the legitimate one.

<img src="https://spacecow99.github.io/path-interception/static/wish_cygpath_search.PNG" width="800" height="300" />

#### Steps to reproduce

1. Locate writable directory in _PATH_.
2. Copy `C:\Windows\System32\calc.exe` to target directory.
3. Rename `calc.exe` to `cygpath.com`.
4. Run the `C:\Program Files (x86)\Git\cmd\git-gui.exe` executable.

<img src="https://spacecow99.github.io/path-interception/static/wish_cygpath_load.PNG" width="800" height="200" />

## NPCAP

### npcapwatchdog Scheduled Task Unquoted Path Interception

The `npcapwatchdog` scheduled task is set to execute `C:\Program Files\Npcap\CheckStatus.bat` at startup. As the path to execute contains a space yet is not quoted, there is a path interception at `C:\Program.exe` that will execute as SYSTEM when the task is run. While creating a file at `C:\Program.exe` requires Administrator priviledges, it allows us a path to escalate to SYSTEM.

<img src="https://Spacecow99.github.io/path-interception/static/npcap_path_interception.PNG" width="604" height="190" />

#### Steps to Reproduce

1. Copy `C:\Windows\System32\calc.exe` to `C:\`.
2. Rename `calc.exe` to `Program.exe`.
3. Reboot or run the `npcapwatchdog` scheduled task.

<img src="https://Spacecow99.github.io/path-interception/static/npcap_scheduled_task.PNG" width="487" height="38" />

## OneDrive

### OneDrive Standalone Update Task Scheduled Task

OneDrive checks for updates by using the per-user scheduled task `\OneDrive Standalone Update Task-<User SID>` which is executed once daily. An attacker can hijack the update binary in the user writable location `%LOCALAPPDATA%\Microsoft\OneDrive\OneDriveStandaloneUpdater.exe` to persist a binary with user permissions. Alternatively, the scheduled task can be launched to obfuscate the execution of the malicious binary.

#### Steps to Reproduce

1. Copy `C:\Windows\System32\calc.exe` to `%LOCALAPPDATA%\Microsoft\OneDrive\OneDriveStandaloneUpdater.exe`
2. Run the `\OneDrive Standalone Update Task-<User SID>` scheduled task.

#### OneDrive Standalone Update Task

An example of the `\OneDrive Standalone Update Task-<User SID>` scheduled task can be found below:

```xml
<?xml version="1.0" encoding="UTF-16"?>
<Task version="1.2" xmlns="http://schemas.microsoft.com/windows/2004/02/mit/task">
  <RegistrationInfo>
    <Author>Microsoft Corporation</Author>
    <URI>\OneDrive Standalone Update Task-S-1-5-21-1234567890-1234567890-1234567890-1000</URI>
  </RegistrationInfo>
  <Triggers>
    <TimeTrigger>
      <Repetition>
        <Interval>P1D</Interval>
        <StopAtDurationEnd>false</StopAtDurationEnd>
      </Repetition>
      <StartBoundary>1992-05-01T21:00:00</StartBoundary>
      <Enabled>true</Enabled>
      <RandomDelay>PT4H</RandomDelay>
    </TimeTrigger>
  </Triggers>
  <Principals>
    <Principal id="Author">
      <UserId>S-1-5-21-1234567890-1234567890-1234567890-1000</UserId>
      <LogonType>InteractiveToken</LogonType>
      <RunLevel>LeastPrivilege</RunLevel>
    </Principal>
  </Principals>
  <Settings>
    <MultipleInstancesPolicy>IgnoreNew</MultipleInstancesPolicy>
    <DisallowStartIfOnBatteries>false</DisallowStartIfOnBatteries>
    <StopIfGoingOnBatteries>true</StopIfGoingOnBatteries>
    <AllowHardTerminate>true</AllowHardTerminate>
    <StartWhenAvailable>true</StartWhenAvailable>
    <RunOnlyIfNetworkAvailable>true</RunOnlyIfNetworkAvailable>
    <IdleSettings>
      <StopOnIdleEnd>true</StopOnIdleEnd>
      <RestartOnIdle>false</RestartOnIdle>
    </IdleSettings>
    <AllowStartOnDemand>true</AllowStartOnDemand>
    <Enabled>true</Enabled>
    <Hidden>false</Hidden>
    <RunOnlyIfIdle>false</RunOnlyIfIdle>
    <WakeToRun>false</WakeToRun>
    <ExecutionTimeLimit>P1D</ExecutionTimeLimit>
    <Priority>7</Priority>
  </Settings>
  <Actions Context="Author">
    <Exec>
      <Command>%localappdata%\Microsoft\OneDrive\OneDriveStandaloneUpdater.exe</Command>
    </Exec>
  </Actions>
</Task>
```

## Signal Desktop Client

### WMIC.exe Path Interception

The [Signal](https://www.signal.org/) Windows desktop application is susceptible to a path interception when attempting to execute the command `wmic os get locale`, causing signal to begin a path search for the binary. This can be abused by moving an executable in to the user writable `%APPDATA%\Local\Programs\signal-desktop`, renaming it to `wmic.exe` and executing Signal. This can allow an attacker to execute or persist a binary that will run when ever Signal is started.

<img src="https://spacecow99.github.io/path-interception/static/signal_wmic_search.PNG" width="550" height="250" />

#### Steps to reproduce

1. Copy `C:\Windows\System32\calc.exe` to `%APPDATA%\Local\Programs\signal-desktop`.
2. Rename `calc.exe` to `wmic.exe`.
3. Open Signal from the start menu or desktop icon.

<img src="https://spacecow99.github.io/path-interception/static/signal_wmic_hijack.PNG" width="700" height="400" />

## Windows Identity Foundation

### Claims to Windows Token Service Unquoted Path Interception

The `Claims to Windows Token Service` (`c2wts`) has an unquoted service path `C:\Program Files\Windows Identity Foundation\v3.5\c2wtshost.exe` that allows for path interception at `C:\Program.exe`, `C:\Program Files\Windows.exe` or `C:\Program Files\Windows Identity.exe`. While creating a file in these locations require Administrator priviledges, it allows us a path to escalate to SYSTEM.

<img src="https://spacecow99.github.io/path-interception/static/c2wts_service_path.PNG" width="573" height="351" />

#### Steps to Reproduce

1. Copy `C:\Windows\System32\calc.exe` to `C:\`.
2. Rename `calc.exe` to `Program.exe`.
3. Start the `c2wts` service.

<img src="https://spacecow99.github.io/path-interception/static/c2wts_program_process.PNG" width="548" height="351" />
