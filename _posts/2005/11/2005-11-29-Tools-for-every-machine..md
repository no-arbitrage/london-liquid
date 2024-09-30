---
title: "Tools for every machine."
date: "2005-11-29T23:32:06"
tags: [
  "technical"
]
---
There are some tools I find myself adding to every machine I work on. Some of these come from Scott Hanselmans’ [Ultimate Tools List](http://www.hanselman.com/blog/ScottHanselmans2005UltimateDeveloperAndPowerUsersToolList.aspx). Currently I install the following on every machine :-

-   [Notepad2](http://www.flos-freeware.ch/notepad2.html)
-   [Winzip](http://www.winzip.com)
-   [CmdHere Powertoy for XP](http://www.microsoft.com/windowsxp/downloads/powertoys/xppowertoys.mspx)
-   [BGinfo from SysInternals](http://www.sysinternals.com/Utilities/BgInfo.html)
-   [CommandBar for Explorer](http://www.codeproject.com/csharp/CommandBar.asp)
-   [VNC](http://www.realvnc.com/download.html)

I also use a bunch of others (Cropper, Magnifixer, Snippet Compiler etc), but the first list is my ‘MUST HAVES’ on every machine. To make it all a bit easier I put together a short script to copy the apps over to the new machine, install all the apps, do the necessary with reg entries etc. Code is as follows :-

Dim objFS, WshShell, link, bDebug, sDestinationFolder

bDebug = false
sDestinationFolder = "C:WindowsKensTools"


Set objFS = CreateObject("Scripting.FileSystemObject")
Set WshShell = CreateObject("WScript.Shell")

' Create the new folders (dont care about errors)
On Error Resume Next
objFS.CreateFolder sDestinationFolder
Err.Clear

' Copy the files over
objFS.CopyFolder ".\*.\*", sDestinationFolder, true
If Err then
	Log ("Failed to copy folder over:" & vbcrlf & vbCrlF & err.description)
	Err.Clear
Else
	if bDebug then Log "Copied folder over"
End If

objFS.CopyFile ".\*.\*", sDestinationFolder, true
If Err then
	Log ("Failed to copy files over:" & vbcrlf & vbCrlF & err.description)
	Err.Clear
Else
	if bDebug then Log "Copied files over"
End If

' Do the apps
InstallApp "regedit.exe /s " & sDestinationFolder & "Notepad2n2\_shell\_integration.reg", "Notepad2"
InstallApp "msiexec /i """ & sDestinationFolder & "CmdHere Powertoy For Windows XP.msi""", "CmdHere Powertoy"
InstallApp sDestinationFolder & "VNC-4\_1\_1-x86\_Win32.exe", "VNC"
InstallApp sDestinationFolder & "Winzip90.exe", "Winzip"
InstallApp "msiexec /i " & sDestinationFolder & "CommandBarSetup.msi", "CommandBar for Explorer"


' Create the BG Info shortcut
Set link = WshShell.CreateShortcut("C:Documents and SettingsAll UsersStart MenuProgramsStartupBGinfo.lnk")
link.Arguments = sDestinationFolder & "kens\_default.bgi /TIMER:3"
link.Description = "BGinfo"
link.TargetPath = sDestinationFolder & "BGInfo.exe"
link.HotKey = "CTRL+ALT+SHIFT+B"
link.IconLocation = sDestinationFolder & "BGInfo.exe,1"
link.WindowStyle = 1
link.WorkingDirectory = sDestinationFolder
link.Save
If Err then
	Log ("Failed to install BGinfo shortcut:" & vbcrlf & vbCrlF & err.description)
	Err.Clear
Else
	if bDebug then Log "Installed BGinfo shortcut"
End If

Set link = nothing
Set WshShell = nothing
Set objFS = nothing




Sub InstallApp(byval cmdLine, byval appName)

	Set oShell = CreateObject("WScript.Shell")
	Set oExec = oShell.Exec(cmdLine)

	Do While oExec.Status = 0
		WScript.Sleep 100
	Loop
	If Err then
		Log ("Failed to install " & appName & " :" & vbcrlf & vbCrlF & err.description)
		Err.Clear
	Else
		if bDebug then Log ("Installed " & appName)
	End If

	Set oExec = nothing
	Set oShell = nothing

end Sub

Sub Log(byval Message)

	Wscript.echo Message

End Sub