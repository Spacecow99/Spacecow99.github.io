
- return [$HOME](https://spacecow99.github.io/)

# Signal Desktop Client

## WMIC.exe Path Interception

The [Signal](https://www.signal.org/) Windows desktop application is susceptible to a path interception when attempting to execute the command `wmic os get locale`, causing signal to begin a path search for the binary. This can be abused by moving an executable in to the user writable `%APPDATA%\Local\Programs\signal-desktop`, renaming it to `wmic.exe` and executing Signal. This can allow an attacker to execute or persist a binary that will run when ever Signal is started.

<img src="https://spacecow99.github.io/path-interception/signal/signal_wmic_search.PNG" width="550" height="250" />

### Steps to reproduce

1. Copy `C:\Windows\System32\calc.exe` to `%APPDATA%\Local\Programs\signal-desktop`.
2. Rename `calc.exe` to `wmic.exe`.
3. Open Signal from the start menu or desktop icon.

<img src="https://spacecow99.github.io/path-interception/signal/signal_wmic_hijack.PNG" width="700" height="400" />
