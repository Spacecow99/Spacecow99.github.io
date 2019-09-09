
- return [$HOME](https://spacecow99.github.io/)

# Windows Identity Foundation

## Claims to Windows Token Service Unquoted Path Interception

The `Claims to Windows Token Service` (`c2wts`) has an unquoted service path `C:\Program Files\Windows Identity Foundation\v3.5\c2wtshost.exe` that allows for path interception at `C:\Program.exe`, `C:\Program Files\Windows.exe` or `C:\Program Files\Windows Identity.exe`. While creating a file in these locations require Administrator priviledges, it allows us a path to escalate to SYSTEM.

<img src="https://spacecow99.github.io/path-interception/windows-identity-foundation/c2wts_service_path.PNG" width="573" height="351" />

### Steps to Reproduce

1. Copy `C:\Windows\System32\calc.exe` to `C:\`.
2. Rename `calc.exe` to `Program.exe`.
3. Start the `c2wts` service.

<img src="https://spacecow99.github.io/path-interception/windows-identity-foundation/c2wts_program_process.PNG" width="548" height="351" />
