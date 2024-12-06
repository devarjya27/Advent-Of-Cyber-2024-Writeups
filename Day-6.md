Today we learn how to analyze malicious behavior using sandbox tools and explore `YARA` rules to detect these malicious behaviors.
+ Firstly to set up the `Endpoint Detection and Response` we type in the following:
  ```
  FLARE-VM 12/06/2024 19:46:38
  PS C:\Users\Administrator > cd C:\Tools
  FLARE-VM 12/06/2024 19:46:50
  PS C:\Tools > .\JingleBells.ps1
  ```
+ Now we run the malware by navigating to `C:\Tools\Malware`, and double-clicking on `MerryChristmas.exe`. And we get the following popup which is the answer to our first question.

   ![image](https://github.com/user-attachments/assets/767e5611-2a32-4640-994d-025c245b2997)
+ Now we use `Floss` to extract obfuscated strings from malware binaries.
  ```
  FLARE-VM 12/06/2024 19:49:25
  PS C:\Users\Administrator > cd C:\Tools\FLOSS
  FLARE-VM 12/06/2024 19:49:38
  PS C:\Tools\FLOSS > floss.exe C:\Tools\Malware\MerryChristmas.exe |Out-file C:\tools\malstrings.txt
  ```
+ First part of the command executes `floss.exe` and scans the malware `MerryChristmas.exe` for strings then the pipe operator `|` redirects the output to `malstrings.txt`.
+ We now open `malstrings.txt` and search for the substring `THM` to get the answer to the second question.

  ![image](https://github.com/user-attachments/assets/3d8698c6-7ce7-4f54-99a3-f9e3efb643e0)
+ Now we use `YARA` on `Sysmon` logs.
  ```
  FLARE-VM 12/06/2024 19:58:18
  PS C:\Users\Administrator > cd C:\Tools
  FLARE-VM 12/06/2024 19:58:25
  PS C:\Tools > get-content C:\Tools\YaraMatches.txt
  Event Time: 12/06/2024 19:47:49
  Event ID: 1
  Event Record ID: 127850
  Command Line: reg  query "HKLM\Software\Microsoft\Windows\CurrentVersion" /v ProgramFilesDir
  YARA Result: SANDBOXDETECTED C:\Users\Administrator\AppData\Local\Temp\2\tmpA08B.tmp
  --------------------------------------
  
  Event Time: 12/06/2024 19:47:49
  Event ID: 1
  Event Record ID: 127849
  Command Line: C:\Windows\system32\cmd.exe /c reg query "HKLM\Software\Microsoft\Windows\CurrentVersion" /v ProgramFilesDir
  YARA Result: SANDBOXDETECTED C:\Users\Administrator\AppData\Local\Temp\2\tmpADEA.tmp
  --------------------------------------
  ```
+ We note down the Event Record ID `127849`.
+ We open `Windows Event Viewer`, navigate to `Applications and Services Logs` -> `Microsoft` -> `Windows` -> `Sysmon` -> `Operational` -> `Filter Current Log`.
+ Now we edit the `XML` `query manually`
  ```
  <QueryList>
  <Query Id="0" Path="Microsoft-Windows-Sysmon/Operational">
    <Select Path="Microsoft-Windows-Sysmon/Operational">
      *[System[(EventRecordID="127849")]]
    </Select>
  </Query>
  </QueryList>
  ```
+ And we get the following `EventData`:

  ![image](https://github.com/user-attachments/assets/c7bc6911-7af5-4802-a653-74bd65ca292e)
