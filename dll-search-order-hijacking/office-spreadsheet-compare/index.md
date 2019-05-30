
- return [$HOME](https://spacecow99.github.io/)

# Office Spreadsheet Compare

## Mso20Win32Client.dll DLL Search Order Hijack

The `SPREADSHEETCOMPARE.EXE` executable that comes as part of Microsoft Office 2016 is susceptible to a DLL search order hijack where it attempts to load the 32bit DLL `Mso20Win32Client.dll` from the user's PATH upon application startup and the contents of DLL_PROCESS_ATTACH will be executed upon load.

<img src="https://spacecow99.github.io/dll-search-order-hijacking/office-spreadsheet-compare/spreadsheetcompare_mso20win32client_search.PNG" width="800" height="300" />

### Steps to reproduce

1. Locate writable directory in _PATH_.
2. Move DLL payload to target directory.
3. Rename the DLL payload to `Mso20Win32Client.dll`.
4. Run `"C:\Program Files (x86)\Microsoft Office\root\client\AppVLP.exe" "C:\Program Files (x86)\Microsoft Office\Root\Office16\DCF\SPREADSHEETCOMPARE.EXE"`.

<img src="https://spacecow99.github.io/dll-search-order-hijacking/office-spreadsheet-compare/spreadsheetcompare_mso20win32client_hijack.PNG" width="450" height="200" />

## ~~c2r32.dll DLL Search Order Hijack~~

_It appears there may be a DLL search order hijack of the c2r32.dll file in the user's PATH but when the file was loaded the application threw an error message and failed to start. Will require additional research before it can be properly exploited._

<img src="https://spacecow99.github.io/dll-search-order-hijacking/office-spreadsheet-compare/spreadsheetcompare_c2r32_error.PNG" width="400" height="200" />
