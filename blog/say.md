# How to Re-implement the say Command on Windows with PowerShell and a Batch File
If you're a Mac or Linux user, you might be familiar with the say command, which allows you to convert written text 
into spoken words. No such thing exists for Windows. Let's re-implement it on Windows using PowerShell and a batch file!

## Using PowerShell
PowerShell is a powerful command-line tool that comes pre-installed with Windows. Using PowerShell allows us to load
useful assemblies from within a simple script, and it's available on all Windows systems. (though it may be blocked by your
Windows Admin)

Here's a PowerShell script that re-implements the say command:

```powershell
[CmdletBinding()]
param (
  [Parameter(Mandatory=$true, Position=0)]
  [string]$Text
)

Add-Type -AssemblyName System.Speech

$synthesizer = New-Object -TypeName System.Speech.Synthesis.SpeechSynthesizer

$synthesizer.Speak($Text)
```
This script defines a parameter named `Text` that represents the text to be converted to speech. It then uses the System.Speech 
assembly to create a SpeechSynthesizer object and use its Speak() method to convert the text to speech.

To use this script as the say command, save it to a file named `say.ps1` somewhere on your path. You can then run the command from PowerShell like this:

```powershell
.\say.ps1 "Hello, world!"
```
This will convert the text "Hello, world!" to speech and play it back using the default system voice.

## Using a Batch File
While PowerShell is a powerful tool, there may be times you'd rather run the command from `cmd.exe`. This batch file of course depends
on the above Powershell script:

```batch
@echo off

setlocal

set SCRIPT_PATH=C:\Path\To\say.ps1

if "%~1"=="" (
  echo Usage: say "text to convert to speech"
  exit /b 1
)

powershell.exe -ExecutionPolicy Bypass -File "%SCRIPT_PATH%" %1
```


To use this batch file as the say command, save it to a file named `say.bat` somewhere on your %PATH%. Replace `C:\Path\To\say.ps1` with 
the actual path to your PowerShell script. You can then run the command from cmd.exe like this:

```batch
say "Hello, world!"
```

Enjoy a decades-old Linux/Mac feature on your Windows machine!
