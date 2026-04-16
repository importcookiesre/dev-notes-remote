

# What is shell ? 

> A shell is a special user program that provides an interface for the user to use operating system services. Shell accepts human-readable commands from users and converts them into something which the kernel can understand.

There are two types of shell : 
1. Command Line Shell :  Text based
	1. Bash
	2. sh
	3. zsh
2. Graphical shell (GUI) : Interface with windows, icons etc.
	1. Windows Explorer
	2. Gnome Shell 
	3. macOs Finder

# Terminal vs Shell vs Console

## Terminal : 

> A **terminal** is a **program or device** that allows users to interact with the shell via a text-based interface.

- Originally a **physical device** (keyboard + screen) connected to a mainframe.
- Today, it’s **software emulation** of that device.

🔧 Examples:
- GNOME Terminal (Linux)
- Terminal.app (macOS)
- Command Prompt / PowerShell Terminal (Windows)


## Shell : 

> A **shell** is a **command-line interpreter** that reads, parses, and executes user commands.
- It is the **software that runs inside the terminal**.
- The shell interprets commands, runs programs, handles variables, and supports scripting.

🔧 Common Shells:
- `bash` (Bourne Again SHell)
- `sh`, `zsh`, `fish`, `ksh`, `csh`
- `powershell` (for Windows)

## Console

> A **console** refers to the **physical or virtual text input/output interface** provided to interact with the system.

- In the old days: a **hardware console** was used to directly interact with a mainframe or early PCs.
- Now: refers to the **system-level text interface** (e.g., Linux virtual console with Ctrl+Alt+F3).

🔧 Examples:
- Linux Virtual Consoles (`tty1`, `tty2`, etc.)
- Windows Console Host (`conhost.exe`)
- Console window that spawns terminals

## Flow : 
You open **GNOME Terminal** → it starts a **bash shell** → you type `ls` → the shell runs it → kernel returns result → displayed in the **terminal**, through the **console system**.



# sample : 

script.sh
```shell
#!/bin/bash  
# this is a comment
echo "hey buddy!"
<
```

`#!/bin/bash` : shebang

**Execute :**  

1. `bash script.sh`
2. ./script.sh
	1. first we have to give it executable permision.
	2. `chmod +x script.sh`