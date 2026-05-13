# Command Prompt (cmd.exe)

Command prompt is the default command line interpreter for the Windows OS. Originally based on `COMMAND.COM` interpreter is DOS. 

> Sometimes, in penetration testing, the PowerShell is locked down or inaccessible through application control. Command line can be used to further elevate the privileges.

**How do we access the CMD?**

1.  `Local Access` : Can be directly accessed through peripherals. 
    - `Windows + r` to bring the prompt.
    - `C:\Windows\System32\cmd.exe` file path.

2. `Remote Access` : It requires to be connected to the same network or have a route through telnet, SSH, PsExec, WinRM, RDP etc. 

# Getting Help

**Default Help Usage**:
```
C:\user>help
```
It will list out *built-in* commands and provides a basic description.

```
C:\user> help <command_name>
```

```
C:\user> ipconfig /?
```
**Where to get help?**
- Microsoft Documentation
- ss64

**Clearing your screen:**
```
C:\user> cls
```

**Checking history of past commands:**
```
doskey /history

systeminfo
ipconfig /all
cls
ipconfig /all
systeminfo
cls
history
help
doskey /history
ping 8.8.8.8
doskey /history
```

We can view a list of commands that were run before our origin command.

**Useful Keys and Commands:**

- F3 : Will retype the entire previous entry to our prompt
- F5 : Pressing F5 multiple times will allow you to cycle through previous commands.
- F7 : Opens an interactive list of previous commands
- F9 : Enters a command to our prompt based on the number specified.
