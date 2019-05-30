
- return [$HOME](https://spacecow99.github.io/)

# Git-GUI

## cygpath.com Path Interception

The `wish.exe` executable that comes packaged with the Windows Git GUI client is susceptible to a path interception where an attacker can take advantage of a user writable path folder to have a non-existent executable with the filename `cygpath.com` executed upon application startup. The `wish.exe` binary is looking to execute the legitimate `C:\Program Files (x86)\Git\usr\bin\cygpath.exe` but is being run with the command line `cygpath --windir`, causing it to begin a path search for the binary. Using the `.com` file extension ensures that the malicious binary will be loaded before the legitimate one.

<img src="path-interception/git-gui/wish_cygpath_search.PNG" width="400" height="200" />

### Steps to reproduce

1. Locate writable directory in _PATH_.
2. Copy `C:\Windows\System32\calc.exe` to target directory.
3. Rename `calc.exe` to `cygpath.com`.
4. Run the `C:\Program Files (x86)\Git\cmd\git-gui.exe` executable.

<img src="path-interception/git-gui/wish_cygpath_load.PNG" width="400" height="200" />
