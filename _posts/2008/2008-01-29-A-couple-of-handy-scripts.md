---
title: "A couple of handy scripts"
date: "2008-01-29T17:56:50"
tags: [
  "productivity",
  "scripting"
]
---
I have been updating some of my ‘magicwords’ for [SlickRun](http://www.bayden.com/SlickRun/) recently. This a great tool for getting focus on a particular task. Instead of having to mess about opening folders, word documents, web sites all in preparation for a task you can enter one ‘magicword’ and have it do all that work for you.[![sr_header](sr_header_thumb.png)](https://kapie.com/content/binary/WindowsLiveWriter/Acoupleofhandyscripts_F84A/sr_header_2.png)

For example when we ([C2C](http://www.c2c.com)) release a new hotfix the process requires :

-   Review of the technical notes / fix details (from a database report)
-   Grab all the relevant files into a .zip package (I really should have this section automated)
-   Update the ‘versions’ xml file that our app checks so that end users get notified of the fix availability
-   Post the zip file containing the hotfix to our support website.

(in fact I really should automate ALL of this)

Anyway, there were a couple of things that I had wanted to do to make [SlickRun](http://www.bayden.com/SlickRun/) a touch better at getting this environment set up for me…

The first was to minimize all current windows (before opening the set of new ones)  
The second was to automatically post form data to a website.

Both of these required a little scripting….

' Minimize all windows to the taskbar
' Ken Hughes
' 23 Jan 2008

Set objShell = CreateObject("Shell.Application")
objShell.MinimizeAll
Set objShell = Nothing

Just run the script for the results….

' HTTP POST script
' Post form data to a url
' Ken Hughes
' 23rd Jan 2008

' Check cmd line args
If (WScript.Arguments.Count <> 2) Then
    ' none - show usage
    Wscript.echo ""
    Wscript.echo "USAGE: httppost.vbs url ""data"""
Else
    ' got them - so post the data
    sURL = Wscript.Arguments(0)
    sFormData = Wscript.Arguments(1)

    Dim objIE
    Set objIE = CreateObject("InternetExplorer.Application")
    objIE.Visible = True
    objIE.Navigate sURL, , , sFormData, "Content-Type: application/x-www-form-urlencoded;"
End If

Run the script with the URL and the post data as command line parameters – for example httppost.vbs [http://yourdomain.com/page.aspx](http://yourdomain.com/page.aspx) “field1=value1&field2=value2”

[](http://11011.net/software/vspaste)

GEO 51.4043197631836:\-1.28760504722595