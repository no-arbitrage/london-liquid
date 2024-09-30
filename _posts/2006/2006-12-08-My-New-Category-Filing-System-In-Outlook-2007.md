---
title: "My New Category Filing System In Outlook 2007"
date: "2006-12-08T18:46:36"
tags: [
  "scripting"
]
---
I always try to keep my Inbox at a manageable level (for me that is less than 50 items). Everything that I have dealt with (or I need for reference) I file in a specific reference folder

> *ASIDE: I know Getting Things Done, GTD, advocates just deleting it but I’m one of those guys that doesn’t like to delete anything – I have no mailbox quota as we dogfood our own Archiving / Mailbox size control application,* [*Archive One*](http://www.c2c.com) *(shameless plugging).*

Anyway, when the method I used for filing was to assign each email a category (or multiple categories) and then run an Outlook macro to copy it to a folder with the same name as the category. In Outlook 2003 assigning a category was a bit of a pain / lengthy process so I wrote a bunch of macros to automate this from the click of a single toolbar button.

Now I have Office 2007, assigning categories is much simpler, so I ditched the ‘Assign Category XXX’ macros and just wrote ‘FileBasedOnCategory’ macros. I needed one to process just the selected item and another for processing all items in my Inbox.

Now categorizing is a simple right click and select and storing a single toolbar button away…

 Here’s the macros I wrote:

Public Const ReferenceFolder \= "zz Reference"

Public Sub CategorizeThis()

    ' Assign a category to the selected objects
    Dim sCurrent As String, obj As Object, objList As Object

    Set objList \= Application.ActiveExplorer.Selection

    For Each obj In objList

        StoreObject obj

    Next

    Set obj \= Nothing
    Set objList \= Nothing

End Sub

Public Sub CategorizeAll()

    ' Assign a category to the selected objects
    Dim sCurrent As String, obj As Object, objList As Object

    Set objList \= Application.ActiveExplorer.CurrentFolder.Items

    For Each obj In objList

        StoreObject obj

    Next

    Set obj \= Nothing
    Set objList \= Nothing

End Sub

Private Sub StoreObject(ByRef obj As Object)

    Dim vCat As Variant, aCats() As String, o As Object, sFolder As String, bOK As Boolean

    bOK \= True

    If obj.Categories <> "" Then

        aCats \= Split(obj.Categories, ",")
        For Each vCat In aCats()

            sFolder \= ReferenceFolder & "" & Trim(CStr(vCat))
            If CreateMAPIFolder(sFolder) Then

                Set o \= obj.Copy
                o.Move GetMAPIFolder(sFolder)

            Else

                MsgBox ("Could not create folder : " & sFolder)
                bOK \= False
                Exit For

            End If

        Next
        If bOK Then obj.Delete

    End If

End Sub

Private Function CreateMAPIFolder(ByVal sFolder As String) As Boolean

    ' Create a MAPIFolder object of the specified name if none exists
    Dim objNS As NameSpace, objParent As MAPIFolder, objChild As MAPIFolder
    Set objNS \= Application.GetNamespace("MAPI")
    Set objParent \= objNS.GetDefaultFolder(olFolderInbox)

    Dim aFolders() As String, vFolder As Variant
    aFolders \= Split(sFolder, "")
    For Each vFolder In aFolders
        On Error Resume Next
        Set objChild \= objParent.Folders(Trim(CStr(vFolder)))
        If Err Then Set objChild \= objParent.Folders.Add(Trim(CStr(vFolder)))
        Err.Clear
        Set objParent \= objChild
    Next
    If (InStr(objChild.FolderPath, sFolder) \> 0) Then
        CreateMAPIFolder \= True
    Else
        CreateMAPIFolder \= False
    End If
    Set objParent \= Nothing
    Set objChild \= Nothing
    Set objNS \= Nothing

End Function

Private Function GetMAPIFolder(ByVal FolderName As String) As Object

    ' Returns a MAPIFolder object of the specified name
    Dim objNS As NameSpace, objParent As MAPIFolder, objChild As MAPIFolder
    Set objNS \= Application.GetNamespace("MAPI")
    Set objParent \= objNS.GetDefaultFolder(olFolderInbox)

    Dim aFolders() As String, vFolder As Variant
    aFolders \= Split(FolderName, "")
    For Each vFolder In aFolders
        Set objChild \= objParent.Folders(CStr(vFolder))
        Set objParent \= objChild
    Next
    Set GetMAPIFolder \= objChild
    Set objParent \= Nothing
    Set objChild \= Nothing
    Set objNS \= Nothing

End Function

[purchase term paper](http://collegepaperz.org/buy-the-cheapest-term-papers-for-college-students/)