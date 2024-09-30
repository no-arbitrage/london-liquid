---
title: "Scripting Exchange Server"
date: "2005-11-30T23:32:19"
tags: [
  "scripting",
  "technical"
]
---
For ‘ad hoc’ scripting of Exchange Server, you just cannot beat Dmitry Streblechenko’s [Outlook Redemption](http://www.dimastr.com/redemption/).

It is basically a set of COM objects (in a DLL) which wrap many of the Outlook / Exchange objects, saving you from having to put some C++ code together to get around the horrible ‘Another application is trying to access Outlook’ security messages (and requiring user interaction).

I had a quick ‘squiz’ at Dimitry’s site and saw the ‘SafXXXXItem’ objects. I didn’t think these would meet my needs so almost wrote my own wrapper for the 2 or 3 particular objects / calls I needed, when I spotted his Redemption Data Objects (RDO), giving full access to the GAL, Public Folders, Mailboxes etc. In the end it did everything I needed and I got my script running in no time (see functions below).

Function AddUserToFolder(byref oFolder, byval oUserID, byval oUserName, byref objSession)

        '\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
        ' oFolder                    Public Folder object
        ' oUserID                    EntryID of the user (from the GAL)
        ' oUserName                Name of the user (from the GAL)
        ' objSession                Session object
        '\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
	Dim sTempUsername
	sTempUsername = ""
	Set objACL = CreateObject("MSExchange.aclobject")
	objACL.CDOItem = oFolder
	Set objFolderACEs = objACL.ACEs

	' delete the user if they exist already
	For each objFolderACE in objFolderACes
		sTempUsername = GetACLEntryName(objSession, objFolderACE.ID)
		if sTempUsername = oUserName then
			Log "Deleting User : " & sTempUsername
			objFolderACEs.Delete objFolderACE.ID
		end if
	next

	' add user if they did not exist
	Log "Adding user : " & oUserName
	Set objNewACE = CreateObject("MSExchange.ACE")
	objNewACE.ID = oUserID
	objNewACE.Rights = "&H7FB"
	objFolderACEs.Add objNewACE
	objACL.Update

	if err then
		AddUserToFolder = false
	else
		AddUserToFolder = true
	end if

	Set objACL = nothing
	Set objNewACE = nothing
	Set objFolderACEs = nothing

End Function


Function GetACLEntryName(byref objSession, byval oACLEntryID)

        '\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
        ' objSession                Session object
        ' oACLEntryID              EntryID from the folders' ACL
        '\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
	Dim sResult
	sResult = ""
	Select Case oACLEntryId
		Case UDefault
			sResult = "Default" & vbTAB & "Default"
		Case UAnonymous
			sResult = "Anonymous" & vbTAB & "Anonymous"
		Case Else
			sResult = objSession.GetAddressEntryFromID(oACLEntryID).Name

	End Select
	GetACLEntryName = sResult

End Function