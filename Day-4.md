On this day we learn to use atomic red team tests to conduct an attack simulation. We also need to understand the `MITRE ATT&CK Framework`. The `MITRE ATT&CK Framework` is a set of techniques that are implemented by real threat actors to simulate what a real attack might look like. The blue team simulates simple attacks from a collection of red team test cases from the `Atomic Red Team Library` that are mapped to the `MITRE ATT&CK Framework`. Our learning objectives for today is to understand the `MITRE ATT&CK Framework` and learn how to do simple test cases from the `Atomic Red Team Library`.

+ Firstly we execute the command `Invoke-AtomicTest -AtomicTechnique T1566.001` on windows powershell to understand about the technique we want to emulate which in this case is `T1566.001`.
+ We then execute `Invoke-AtomicTest -AtomicTechnique T1566.001 -ShowDetails` to know the details of each test case included in the Atomic.
+ Then we are asked to execude TestNumber 1 which we can do so by executing `Invoke-AtomicTest T1566.001 -TestNumbers 1`.
+ Now we open `Sysmon` i.e. System Monitor and look at the event logs. First we clear it by heading into the directory specified and clear it. And we run the emulation again.
+ If we look at the event details of the log we can see that a process was created for powershell executing the following command `"powershell.exe" & {$url = 'http://localhost/PhishingAttachment.xlsm' Invoke-WebRequest -Uri $url -OutFile $env:TEMP\PhishingAttachment.xlsm}` Thus creates a file called `PhishingAttachment.xlsm` in the Current Directory which is `C:\Users\Administrator\AppData\Local\Temp\` in this case.

## What was the flag found in the .txt file that is found in the same directory as the PhishingAttachment.xslm artefact?
Opening the file `PhishingAttachment.txt` in the specified directory we get the answer which is `THM{GlitchTestingForSpearphishing}`

+ Now we clear the artefacts by invoking the following command `Invoke-AtomicTest T1566.001-1 -cleanup`

## What ATT&CK technique ID would be our point of interest?
Looking up ATT&CK ID for `command and scripting interpreter` we get the answer `T1059`.

## What ATT&CK subtechnique ID focuses on the Windows Command Shell?
Now again looking up subtechnique ID for T1059 that focuses on the Windows Command Subshell we get the answer `T1059.003`.

## What is the name of the Atomic Test to be simulated?
If we execute `Invoke-AtomicTest -AtomicTechnique T1059.003 -ShowDetails` in our powershell we get the details for all the atomic tests and lokoing through them we can see that test number `4` deals with `Simulate BlackByte Ransomware Print Bombing` thus that is our answer for this question.

## What is the name of the file used in the test?
Looking through the details we see that file used in this test is `Wareville_Ransomware.txt` in the section for Test Number 4.

## What is the flag found from this Atomic Test?
Now we run the atomic by doing `Invoke-AtomicTest -AtomicTechnique T1059.003 --TestNumbers 4` and then we get a popup asking to save `Wareville_Ransomeware.txt` as a pdf. We save it and after opening the same we get our required flag. which is `THM{R2xpdGNoIGlzIG5vdCB0aGUgZW5lbXk=}`.

(Decoding the flag which seems to be in base64 we get the output `Glitch is not the enemy`)

## What I Learned
+ The Blue team at times simulate test cases from the Atomic Red Team Library to reduce the gaps in their detection and strengthen their defences and be prepared for the day when a real attacker tries to attack their systems.
