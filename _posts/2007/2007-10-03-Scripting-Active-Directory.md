---
title: "Scripting Active Directory"
date: "2007-10-03T21:41:30"
tags: [
  "scripting"
]
---
I seem to have been dong a lot of scripting work recently – I really should be using PowerShell, but I had limited time to get this stuff done and it needed to pretty generic / simple as it would be implemented by others (some with limited knowledge of scripting / coding)..

Anyway, the requirement was to ‘shlurp’ all the users from areas of Active Directory for use with one of our products. I wanted to get all users in specific AD containers into a dictionary object so that I could use them later…

The containers I looked at were :-

-   Organizational Units (OUs) – needed to be recursive to pull in sub OUs
-   Distribution Lists (DLs) – also needed to be recursive to pull in sub DLs
-   Query Based Distribution Lists
-   Users with a mailbox on a particular server.

The code is below. One of the keys to this is using LDP.exe to get the LDAP DNs for the various containers/lists….

Set objDictionary = CreateObject("Scripting.Disctionary")
GetAllMembersFromOU "OU=Reading,OU=UK,DC=YOURDOMAIN,DC=COM", objDictionary
GetAllMembersFromDL "DN=UK\_Sales,OU=UK,DC=YOURDOMAIN,DC=COM", objDictionary
GetAllMembersFromQueryBasedDL "DN=CRMUsers,OU=UK,DC=YOURDOMAIN,DC=COM", objDictionary  
  
strHomeServer = "/o=ABC Systems/ou=First Administrative Group/cn=Configuration/cn=Servers/cn=EX-01"  
GetAllMembersFromServer strHomeServer, objDictionary

' Write the found users out to a text file  
WriteDictionaryToTextFile objDictionary, "c:ad\_users.txt"

‘ BELOW ARE THE FUNCTIONS THAT DO THE WORK

'\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
' GetAllMembersFromOU
'
' Gets all the members of an OU (including sub OUs) and adds them to a dictionary object
'
' sDN - a string containing the DN of the OU to start from (e.g. "LDAP://OU=Reading,OU=UK,DC=YOURDOMAIN,DC=COM")
' dic - a 'Scripting.Dictionary' object that you want to hold all the objects in.
'
'
' Ken Hughes 10 July 2007
'\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
Function GetAllMembersFromOU(sDN, dic)

    Set objOU = GetObject("LDAP://" & sDN)

    For each objMember in ObjOU

        Select Case LCase(objMember.Class)

            case "organizationalunit"
                GetAllMembersFromOU objMember.ADSPath, dic

            case "user"
                AddUserToDictionary dic, objMember

            case else
                ' do nothing it was a strange class
        End Select

    Next

End Function

'\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
' GetAllMembersFromDL
'
' Gets all the members of a distribution list (including sub DLs) and adds them to a dictionary object
'
' sDN - a string containing the DN of the OU to start from (e.g. "LDAP://OU=Reading,OU=UK,DC=YOURDOMAIN,DC=COM")
' dic - a 'Scripting.Dictionary' object that you want to hold all the objects in.
'
'
' Ken Hughes 10 July 2007
'\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
Function GetAllMembersFromDL(sDL, dic)

    Set objOU = GetObject("LDAP://" & sDL)

    For each objMember in ObjOU.Members

        Select Case LCase(objMember.Class)

            case "group"
                GetAllMembersFromDL objMember.ADSPath, dic

            case "user"
                AddUserToDictionary dic, objMember

            case else
                ' do nothing it was a strange class

        End Select

    Next

End Function

'\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
' GetAllMembersFromServer
'
' Gets all the users who have mailboxes homed on a particular server
'
' sDN - a string containing the msExchHomeServerName   
'      (e.g. "LDAP:///o=ABC Systems/ou=First Administrative Group/cn=Configuration/cn=Servers/cn=EX-01")
' dic - a 'Scripting.Dictionary' object that you want to hold all the objects in.
'
'
' Ken Hughes 10 July 2007
'\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
Function GetAllMembersFromServer(sDN, dic)

    set conn = createobject("ADODB.Connection")
    Set iAdRootDSE = GetObject("LDAP://RootDSE")
    strDefaultNamingContext = iAdRootDSE.Get("defaultNamingContext")
    Conn.Provider = "ADsDSOObject"
    Conn.Open "ADs Provider"

    strQueryDL = "<LDAP://" & strDefaultNamingContext & ">;(&(msExchHomeServerName=" & sDN   
    strQueryDL = strQueryDL & ")(objectCategory=person)(objectClass=user));distinguishedName,adspath;subtree"
    set objCmd = createobject("ADODB.Command")
    objCmd.ActiveConnection = Conn
    objCmd.Properties("SearchScope") = 2 ' we want to search everything
    objCmd.Properties("Page Size") = 500 ' and we want our records in lots of 500

    objCmd.CommandText = strQueryDL
    Set objRs = objCmd.Execute

    While Not objRS.eof

        Set objMember = GetObject("LDAP://" & replace(objRS.Fields("distinguishedName"),"/","/"))

            Select Case LCase(objMember.Class)

                case "user"
                    AddUserToDictionary dic, objMember

                case else
                    ' do nothing it was a strange class

            End Select

        objRS.MoveNext
    Wend

End Function

'\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
' GetAllMembersFromQueryBasedDL
'
' Gets all the members of a query based distribution list
'
' sDN - a string containing the DN of the QBDL (e.g. "LDAP://DL=AdminUsers,OU=Reading,OU=UK,DC=YOUROMAIN,DC=COM")
' dic - a 'Scripting.Dictionary' object that you want to hold all the objects in.
'
'
' Ken Hughes 10 July 2007
'\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
Function GetAllMembersFromQueryBasedDL(sDN, dic)

    set conn = createobject("ADODB.Connection")
    Set iAdRootDSE = GetObject("LDAP://RootDSE")
    strDefaultNamingContext = iAdRootDSE.Get("defaultNamingContext")
    Conn.Provider = "ADsDSOObject"
    Conn.Open "ADs Provider"

    Set objQBDL = GetObject("LDAP://" & sDN)

    strQueryDL = "<LDAP://" & objQBDL.msExchDynamicDLBaseDN & ">;" & objQBDL.msExchDynamicDLFilter  
    strQueryDL = strQueryDL & ";mail,ObjectClass,distinguishedName,displayname,legacyExchangeDN,homemdb;subtree"
    set objCmd = createobject("ADODB.Command")
    objCmd.ActiveConnection = Conn
    objCmd.Properties("SearchScope") = 2 ' we want to search everything
    objCmd.Properties("Page Size") = 500 ' and we want our records in lots of 500

    objCmd.CommandText = strQueryDL
    Set objRs = objCmd.Execute

    While Not objRS.eof

        Set objMember = GetObject("LDAP://" & replace(objRS.Fields("distinguishedName"),"/","/"))

            Select Case LCase(objMember.Class)

                case "user"
                    AddUserToDictionary dic, objMember

                case else
                    ' do nothing it was a strange class

            End Select

        objRS.MoveNext
    Wend

End Function
'\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
' WriteDictionaryToTextFile
'
' Writes the contents of a dictionary object to a text file
'
' objDic   - a 'Scripting.Dictionary' object that holds all the objects you want written to the file
' fileName - a string containing the name of the file you want the objects written out to.
'
'
' Ken Hughes 10 July 2007
'\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
Sub WriteDictionaryToTextFile(objDic, fileName)

    Dim objFSO, objFile
    Const forReading = 1
    Const forWriting = 2
    Const forAppending = 8

    Set objFSO = CreateObject("Scripting.FileSystemObject")
    Set objFile = objFSO.OpenTextFile(filename, forWriting, True)

    For Each obj in objDic.Keys
        objFile.Writeline obj
    Next
    objFile.Close
    Set objFile = Nothing
    Set objFSO = Nothing
End Sub

GEO 51.4043197631836:\-1.28760504722595

[](http://11011.net/software/vspaste)