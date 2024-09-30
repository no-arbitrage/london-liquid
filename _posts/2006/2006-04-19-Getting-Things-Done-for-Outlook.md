---
title: "Getting Things Done for Outlook"
date: "2006-04-19T16:57:39"
tags: [
  "scripting",
  "technical"
]
---
OK, so I’ve been modifying my (personal) organisational processes recently – how I track my tasks / projects etc…

I’ve kind of moved over to Getting Things Done (GTD) by [David Allen](http://www.davidco.com/). It’s quick and easy and gets rid of all the ‘non critical’ crap that you really don’t need to keep in your (short term cache) head – allowing you to focus what’s important.

Anyway, in addition to understanding an organisational process, there is an element of making it work in your own environment (e.g. configuring your laptop / desktop with the tools to allow you to follow the process easily). There are a number of reviews etc of the Getting Things Done Add In for Outlook by Netcentrics ([http://gtdsupport.netcentrics.com/home/](http://gtdsupport.netcentrics.com/home/ "http://gtdsupport.netcentrics.com/home/")). It seems like a pretty good application, but being a bit of a miser / skinflint and having a tendancy to play around with coding / scripting, I decided I didn’t want to spend $70 and that I would hack something together myself.

What I came up with is a set of fairly simple Outlook macros, but it covers all the aspects of GTD that I use. The macros cover :-

-   **Make Task** – This takes an email and generates a new task. The task gets the body of the email copied into it and it also gets a copy of the email embedded into it.
-   **Schedule** – This takes an email and generates a new appointment. The appointment gets the body of the email copied into it and also gets a copy of the email embedded into it.
-   **Delegate Via Task** – This takes an email and generates a new task. The task gets the body of the email copied into it and it also gets a copy of the email embedded into it. It also starts the ‘Assign Task to User’ process.
-   **Delegate Via Email** – This takes an email and generates a new task with the original email subject preceded by ‘FOLLOW UP’. The task gets the body of the email copied into it and it also gets a copy of the email embedded into it and the line ‘Follow up on task sent by email (Date) to XXXXX’ appended. It then also starts the ‘Forward Email’ process. The original email is ‘Stored for Reference’
-   **Store For Ref** – This Copies the email into a subfolder of the ‘Reference’ folder with the name of the category assigned to the email. If the email is assigned multiple categories then multiple copies are stored (one in each subfolder)

There are also a bunch of other macros that allow a specific category to be assigned to the item. These are ObjectCat\_\[CategoryName\] or TableCat\_\[CategoryName\]. I simply add the ones I use most so they are available from the toolbar.

The reason there are (seemingly) duplicated functions (one starting with ObjectXXXXXX the other being TableXXXXXX) is that we need to provide the features whether we are in ‘tableview’ (and possibly have multiple emails selected) or if we have the email object itself open

Anyway, here is the code :

' Ken Hughes
' 21 March 2006
' Macros to implement 'Getting Things Done'

' Config variables
Const ReferenceFolder = "Reference"
Const TempFileName = "GTD\_Temp\_Msg.msg"

*These are the constants that I use within my system / folder set – you may have to customise these to your own environment. Reference is the top level folder that all my ‘stored for reference’ subfolders are under (e.g. Development, Helpdesk, Personnel etc)*

' For the currently open object, display the categories dialog.
' No need for a Table version of this as you can simple highlight the items in the table view,   
' right click and choose Categories...
Public Sub ObjectCategorise()

    ' Shows the categories dialog for the item.

    Dim obj As Object

    Set obj = Application.ActiveInspector.CurrentItem
    obj.Display
    obj.ShowCategoriesDialog
    obj.Save
    obj.Close 2

    Set obj = Nothing

End Sub

' Takes each highlighted email in the tableview and creates a task for it.   
' Also copies the email body into the task body and attaches the email object into the task body.
Public Sub TableMakeTask()
    ' Convert an email into a task
    Dim objEmail As MailItem, objNewTask As TaskItem

    For Each objEmail In Application.ActiveExplorer.Selection
        Set objNewTask = CreateTaskFromEmail(objEmail)
        objEmail.Move GetMAPIFolder(ReferenceFolder)
        objNewTask.Display
    Next

    Set objEmail = Nothing
    Set objNewTask = Nothing

End Sub

' Takes the currently open email object and creates a task for it.   
' Also copies the email body into the task body and attaches the email object into the task body.
Public Sub ObjectMakeTask()
    ' Convert an email into a task
    Dim objEmail As MailItem, objNewTask As TaskItem

    Set objEmail = Application.ActiveInspector.CurrentItem
    Set objNewTask = CreateTaskFromEmail(objEmail)
    objEmail.Move GetMAPIFolder(ReferenceFolder)
    objNewTask.Display

    Set objEmail = Nothing
    Set objNewTask = Nothing

End Sub

' Takes each highlighted email in the tableview and creates an appointment for it.
' Also copies the email body into the appointment body and attaches the email object   
' into the appointment body.
Public Sub TableSchedule()
    ' Convert an email into an appointment
    Dim objEmail As MailItem, objNewAppt As AppointmentItem

    For Each objEmail In Application.ActiveExplorer.Selection
        Set objNewAppt = CreateApptFromEmail(objEmail)
        objEmail.Move GetMAPIFolder(ReferenceFolder)
        objNewAppt.Display
    Next

    Set objEmail = Nothing
    Set objAppt = Nothing

End Sub

' Takes the currently open emailobject and creates an appointment for it.
' Also copies the email body into the appointment body and attaches the email   
' object into the appointment body.
Public Sub ObjectSchedule()
    ' Convert an email into an appointment
    Dim objEmail As MailItem, objNewAppt As AppointmentItem

    Set objEmail = Application.ActiveInspector.CurrentItem
    Set objNewAppt = CreateApptFromEmail(objEmail)
    objEmail.Move GetMAPIFolder(ReferenceFolder)
    objNewAppt.Display

    Set objEmail = Nothing
    Set objAppt = Nothing

End Sub

' Takes each highlighted email in the tableview and creates an assigned task for it.
' Also copies the email body into the task body and attaches the email object into the task body.
Public Sub TableDelegateViaTask()
    ' Creates a task from an email and lets user assign owner
    Dim objEmail As MailItem, objNewTask As TaskItem

    For Each objEmail In Application.ActiveExplorer.Selection
        Set objNewTask = CreateTaskFromEmail(objEmail)
        objEmail.Move GetMAPIFolder(ReferenceFolder)
        objNewTask.Assign
        objNewTask.Display
    Next

    Set objEmail = Nothing
    Set objNewTask = Nothing

End Sub

' Takes the currently open email object and creates an assigned task for it.
' Also copies the email body into the task body and attaches the email object into the task body.
Public Sub ObjectDelegateViaTask()
    ' Creates a task from an email and lets user assign owner
    Dim objEmail As MailItem, objNewTask As TaskItem

    Set objEmail = Application.ActiveInspector.CurrentItem
    Set objNewTask = CreateTaskFromEmail(objEmail)
    objEmail.Move GetMAPIFolder(ReferenceFolder)
    objNewTask.Assign
    objNewTask.Display

    Set objEmail = Nothing
    Set objNewTask = Nothing

End Sub

' Takes each highlighted email in the tableview and creates a task for it (that you track yourself)
' and also forwards the original email (allowing you to delegate it via email).
' Also copies the email body into the task body and attaches the email object into the task body.
Public Sub TableDelegateViaEmail()
    ' Creates a personal task from an email and forwards the original email
    Dim objEmail As MailItem, objNewTask As TaskItem

    For Each objEmail In Application.ActiveExplorer.Selection
        Set objNewTask = CreateTaskFromEmail(objEmail)
        objEmail.Move GetMAPIFolder(ReferenceFolder)
        objNewTask.Subject = "FOLLOW UP: " & objNewTask.Subject
        objNewTask.Body = "   Follow up on task sent by email (" & Date & ") to XXXXX"
        Set objEmail = objEmail.Forward
        objEmail.Display
        objNewTask.Display
    Next

    Set objEmail = Nothing
    Set objNewTask = Nothing

End Sub

' Takes each highlighted email in the tableview and creates a task for it (that you track yourself)
' and also forwards the original email (allowing you to delegate it via email).
' Also copies the email body into the task body and attaches the email object into the task body.
Public Sub ObjectDelegateViaEmail()
 ' Creates a personal task from an email and forwards the original email
 
    Dim objEmail As MailItem, objNewTask As TaskItem

    Set objEmail = Application.ActiveInspector.CurrentItem
    Set objNewTask = CreateTaskFromEmail(objEmail)
    objEmail.Move GetMAPIFolder(ReferenceFolder)
    objNewTask.Subject = "FOLLOW UP: " & objNewTask.Subject
    objNewTask.Body = "   Follow up on task sent by email (" & Date & ") to XXXXX"
    Set objEmail = objEmail.Forward
    objEmail.Display
    objNewTask.Display

    Set objEmail = Nothing
    Set objNewTask = Nothing

End Sub

Public Sub TableStoreForReference()

    ' Stores the email object in the reference folder
    Dim obj As Object

    For Each obj In Application.ActiveExplorer.Selection
        StoreObject obj
    Next

    Set obj = Nothing

End Sub

Public Sub ObjectStoreForReference()

    ' Stores the email object in the reference folder
    Dim obj As Object

    Set obj = Application.ActiveInspector.CurrentItem
    StoreObject obj

    Set obj = Nothing

End Sub

These following macros are the ‘quick access’ macros I use to assign categories to a email object (so it can be stored in the appropriate place)

Public Sub ObjectCat\_Blank()

    ' Assigns the specified category to the object    ObjectAssignCategory ""

End Sub


Public Sub TableCat\_Blank()    ' Assigns the specified category to the object    TableAssignCategory ""

End Sub



Public Sub ObjectCat\_IT()
    ' Assigns the specified category to the object    ObjectAssignCategory "IT"

End Sub


Public Sub TableCat\_IT()
    ' Assigns the specified category to the object    TableAssignCategory "IT"

End Sub


Public Sub ObjectCat\_Helpdesk()    ' Assigns the specified category to the object    ObjectAssignCategory "Helpdesk"

End Sub


Public Sub TableCat\_Helpdesk()
    ' Assigns the specified category to the object    TableAssignCategory "Helpdesk"

End Sub


Public Sub ObjectCat\_Development()
    ' Assigns the specified category to the object    ObjectAssignCategory "Development"

End Sub


Public Sub TableCat\_Development()
    ' Assigns the specified category to the object    TableAssignCategory "Development"

End Sub


Public Sub ObjectCat\_Sales()    ' Assigns the specified category to the object    ObjectAssignCategory "Sales"

End Sub


Public Sub TableCat\_Sales()
    ' Assigns the specified category to the object    TableAssignCategory "Sales"

End Sub


Public Sub ObjectCat\_Partners()
    ' Assigns the specified category to the object ObjectAssignCategory "Partners"

End Sub


Public Sub TableCat\_Partners()
    ' Assigns the specified category to the object    TableAssignCategory "Partners"

End Sub


Public Sub ObjectCat\_Customers()
    ' Assigns the specified category to the object    ObjectAssignCategory "Customers"

End Sub


Public Sub TableCat\_Customers()
    ' Assigns the specified category to the object    TableAssignCategory "Customers"

End Sub

Private Sub TableAssignCategory(ByVal sCat As String)
    ' Assign a category to the object 
    Dim sCurrent As String, obj As Object

    For Each obj In Application.ActiveExplorer.Selection
        sCurrent = obj.Categories
        If sCat = "" Then
            sCurrent = ""
        Else
            If sCurrent <> "" Then
                sCurrent = sCurrent & ", " & sCat
            Else
                sCurrent = sCat
            End If
        End If
        obj.Categories = sCurrent
        obj.Save
    Next

    Set obj = Nothing

End Sub


Private Sub ObjectAssignCategory(ByVal sCat As String)

    ' Assign a category to the object
    Dim sCurrent As String, obj As Object

    Set obj = Application.ActiveInspector.CurrentItem
    sCurrent = obj.Categories
    If sCat = "" Then
        sCurrent = ""
    Else
        If sCurrent <> "" Then
            sCurrent = sCurrent & ", " & sCat
        Else
            sCurrent = sCat
        End If
    End If
    obj.Categories = sCurrent
    obj.Save

    Set obj = Nothing

End Sub

This final macro does the filing of the email objects based on the categories they have been assigned

Public Sub DeferItem(ByRef objEmail As MailItem)

    ' Stores the email object in the reference folder    Dim objEmail As MailItem, frm As frmDefer, sInterval As String, iInterval As Integer
    Set frm = New frmDefer

    For Each objEmail In Application.ActiveExplorer.Selection

        frm.Show vbModal
        Select Case LCase(frm.Result)

            Case "someday"
                ' move it to the 'someday' folder
                objEmail.Move GetMAPIFolder(SomedayFolder)

            Case "cancel"
                ' do nothing

            Case Else
                ' add a reminder to the email
                objEmail.Move GetMAPIFolder(DeferredFolder)
                objEmail.FlagRequest = "Follow up"
                sInterval = Right(frm.Result, 1)
                iInterval = CInt(Left(frm.Result, 1))
                objEmail.FlagDueBy = DateAdd(sInterval, iInterval, Now())
                objEmail.FlagIcon = olRedFlagIcon
                objEmail.FlagStatus = olFlagMarked

        End Select

    Next


End Sub

Private Functions used as helpers

Private Function CreateTaskFromEmail(ByRef objEmail As MailItem) As TaskItem
    ' Creates a task from an email object.    Dim objTask As TaskItem
    Set objTask = Application.CreateItem(olTaskItem)
    objTask.Subject = objEmail.Subject
    objTask.Body = "     " & vbCrLf & objEmail.Body
    objEmail.SaveAs (GetTempFolderPath & "" & TempFileName)
    objTask.Attachments.Add GetTempFolderPath & "" & TempFileName, , 1
    Set CreateTaskFromEmail = objTask

End Function

Private Function CreateApptFromEmail(ByRef objEmail As MailItem) As AppointmentItem
    ' Creates an appointment from an email object.
    Dim objAppt As AppointmentItem
    Set objAppt = Application.CreateItem(olAppointmentItem)
    objAppt.Subject = objEmail.Subject
    objAppt.Body = "     " & vbCrLf & objEmail.Body
    objEmail.SaveAs (GetTempFolderPath & "" & TempFileName)
    objAppt.Attachments.Add GetTempFolderPath & "" & TempFileName, , 1
    Set CreateApptFromEmail = objAppt

End Function

Private Function GetMAPIFolder(ByVal FolderName As String) As Object
    ' Returns a MAPIFolder object of the specified name    Dim objNS As NameSpace, objParent As MAPIFolder, objChild As MAPIFolder
    Set objNS = Application.GetNamespace("MAPI")
    Set objParent = objNS.GetDefaultFolder(olFolderInbox)

    Dim aFolders() As String, vFolder As Variant
    aFolders = Split(FolderName, "")
    For Each vFolder In aFolders
        Set objChild = objParent.Folders(CStr(vFolder))
        Set objParent = objChild
    Next
    Set GetMAPIFolder = objChild
    Set objParent = Nothing
    Set objChild = Nothing
    Set objNS = Nothing

End Function

Private Function GetTempFolderPath() As String
    ' Returns a string value indicating the temporary public folder path    Dim objFS As FileSystemObject, objTemp As Scripting.Folder

    Set objFS = New FileSystemObject
    Set objTemp = objFS.GetSpecialFolder(2)    'returns the path found in the TMP environment variable    GetTempFolderPath = objTemp.Path
    Set objTemp = Nothing
    Set objFS = Nothing

End Function


Private Sub StoreObject(ByRef obj As Object)

    Dim vCat As Variant, aCats() As String, o As Object, sFolder As String, bOK As Boolean

    bOK = True
    If obj.Categories <> "" Then
        aCats = Split(obj.Categories, ",")
        For Each vCat In aCats()
            sFolder = ReferenceFolder & "" & Trim(CStr(vCat))
            If CreateMAPIFolder(sFolder) Then
                Set o = obj.Copy
                o.Move GetMAPIFolder(sFolder)
            Else
                MsgBox ("Could not create folder : " & sFolder)
                bOK = False
                Exit For
            End If
        Next
        If bOK Then obj.Delete
    End If

End Sub

Private Function CreateMAPIFolder(ByVal sFolder As String) As Boolean
    ' Create a MAPIFolder object of the specified name if none exists    Dim objNS As NameSpace, objParent As MAPIFolder, objChild As MAPIFolder
    Set objNS = Application.GetNamespace("MAPI")
    Set objParent = objNS.GetDefaultFolder(olFolderInbox)

    Dim aFolders() As String, vFolder As Variant
    aFolders = Split(sFolder, "")
    For Each vFolder In aFolders
        On Error Resume Next
        Set objChild = objParent.Folders(Trim(CStr(vFolder)))
        If Err Then Set objChild = objParent.Folders.Add(Trim(CStr(vFolder)))
        Err.Clear
        Set objParent = objChild
    Next
    If (InStr(objChild.FolderPath, sFolder) > 0) Then
        CreateMAPIFolder = True
    Else
        CreateMAPIFolder = False
    End If
    Set objParent = Nothing
    Set objChild = Nothing
    Set objNS = Nothing

End Function

[modGTD.bas (12.72 KB)](https://kapie.com/content/binary/modGTD.bas)  
[guidessay.com](http://guidessay.com/essay-help/)