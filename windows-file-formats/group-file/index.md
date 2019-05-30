
- return [$HOME](https://spacecow99.github.io/)

# Group File

A .GROUP file is an XML file representing a grouping of contact files and can only exist under the special `C:\Users\%USER%\Contacts\` folder. More detail on Contact Groups and the `C:\Users\%USER%\Contacts\` folder can be found [here](https://www.howtogeek.com/202712/the-windows-contacts-folder-what-is-it-and-do-you-need-it/).

## Website Field Command Execution

A [command execution](https://packetstormsecurity.com/files/151194/Microsoft-Windows-.contact-Arbitrary-Code-Execution.html) is present in the .GROUP file format, in the _Website:_ text box under the _Contact Group Details_ tab or in the equivalent `<c:Url><c:Value>` xml element, by specifying the path to an executable. When the _Go_ button is clicked, it causes `wab.exe` to launch the file specified in the _Website:_ text box. Should the target path be a full path, it must not contain a drive letter (i.e. `\Windows\System32\calc.exe`).

<img src="https://spacecow99.github.io/windows-file-formats/group-file/contact_group_details.PNG" width="400" height="200" />