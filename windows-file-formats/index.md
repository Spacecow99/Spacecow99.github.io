
- return [$HOME](https://spacecow99.github.io/)

# Windows File Formats

A list of interesting File Formats native to Windows that present some sort of abusable feature. File format must be supported out-of-box by a vanilla Windows installation.

## Group File

A .GROUP file is an XML file representing a grouping of contact files that can only be created in the special `C:\Users\%USER%\Contacts\` folder. For an overview of the Contact Group filetype's usage and the special `Contacts\` folder, consult this [How-To Geek article](https://www.howtogeek.com/202712/the-windows-contacts-folder-what-is-it-and-do-you-need-it/).

### Website Field Command Execution

A [command execution](https://packetstormsecurity.com/files/151194/Microsoft-Windows-.contact-Arbitrary-Code-Execution.html) can be triggered in the .GROUP file format in either the _Website:_ text box under the _Contact Group Details_ tab or in the equivalent `<c:Url><c:Value>` xml element by specifying the path to an executable rather than a website URL. When the _Go_ button is pressed, it causes `wab.exe` to launch the file specified in the _Website:_ text box. Should the path specified be a full path, it must not contain a drive letter (`\Windows\System32\calc.exe`).

<img src="https://spacecow99.github.io/windows-file-formats/static/contact_group_details.PNG" width="500" height="400" />
