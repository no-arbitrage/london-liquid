---
title: "Outlook RSS and downloading podcasts"
date: "2007-02-21T18:13:07"
tags: [
  "scripting"
]
---
Today, I wanted to configure an Outlook 2007 rule to process RSS items as they are received. By process I mean I wanted to check for attachments/enclosures, if it had any MP3s attached/enclosed then save them to a folder on my desktop (that way I simply sync my [Creative Zen Vision: M](http://www.creative.com/products/product.asp?category=213&subcategory=214&product=14331) with the desktop folder and I have all my podcasts available for my commute).

 I could not seem to get the Outlook rules engine working correctly on RSS feeds, I wanted to run a script (a function I’d written in Outlook VBA) that was executed every time I receive a RSS item.  
The ‘Run A Script’ action in the rules wizard will only list subroutines with a signature like this:

Sub Foo(objItem As MailItem)

(Of course the subroutine name doesn’t matter…)

Anyway this doesn’t work for RSS items as their message class is **IPM.Post.RSS (making them a PostItem object).** So this is a bit of a pain and means I cannot process RSS items using a rule.  
In the end I had to use the following code to do it – I just open the folder of the RSS feed and run this (via a custom toolbar button) and it does it all for me.

Public Sub SaveRSSEnclosures()

    Dim att As Attachment, objList As Object, objItem As Object, iCount As Integer

    Set objList \= Application.ActiveExplorer.CurrentFolder.Items
    iCount \= 0

    For Each objItem In objList

       If (objItem.Attachments.Count \> 0) And (objItem.UnRead) Then

           ' iterate through looking for .mp3's
           For Each att In objItem.Attachments

               If LCase(Right(att.FileName, 3)) \= "mp3" Then

                   ' save it
                   att.SaveAsFile ("c:Userskenhdesktoppodcasts" & att.FileName)
                   iCount \= iCount + 1

               End If

           Next

        End If

    Next

    If iCount \> 0 Then MsgBox "Saved " & iCount & " mp3 items"

End Sub