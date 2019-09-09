
- return [$HOME](https://spacecow99.github.io/)

# OneDrive

## OneDrive Standalone Update Task Scheduled Task

OneDrive checks for updates by using the per-user scheduled task `\OneDrive Standalone Update Task-<User SID>` which is executed once daily. An attacker can hijack the update binary in the user writable location `%LOCALAPPDATA%\Microsoft\OneDrive\OneDriveStandaloneUpdater.exe` to persist a binary with user permissions. Alternatively, the scheduled task can be launched to obfuscate the execution of the malicious binary.

### Steps to Reproduce

1. Copy `C:\Windows\System32\calc.exe` to `%LOCALAPPDATA%\Microsoft\OneDrive\OneDriveStandaloneUpdater.exe`
2. Run the `\OneDrive Standalone Update Task-<User SID>` scheduled task.

### OneDrive Standalone Update Task

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
