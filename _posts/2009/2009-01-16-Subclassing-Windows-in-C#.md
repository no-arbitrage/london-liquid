---
title: "Subclassing Windows in C#"
date: "2009-01-16T17:06:21"
tags: [
  "development"
]
---
[![image](image_thumb_1.png)](https://kapie.com/content/binary/WindowsLiveWriter/SubclassingWindowsinC_E70A/image_4.png) I’ve been toying around with creating an Outlook addin recently that adds a new panel to the main Outlook inspector window. Actually it is going to be a ‘folding’ or ‘collapsing’ windows similar to the docked ToolWindows in Visual Studio.[![image](image_thumb_2.png)](https://kapie.com/content/binary/WindowsLiveWriter/SubclassingWindowsinC_E70A/image_6.png)

Anyway, the stuff around getting the correct window handle for the Outlook inspector window and resizing it to allow some space to insert my new panel was fairly simple, but I also had to hook into the message look of the window so that I could capture any **move** or **resize** messages and so resize things again to accommodate my panel.

I had started developing it as an Outlook add-in but to speed things up (and prevent me having to register / unregister the addin between tests etc I decided to develop it as a separate app- I could still hook into Outlook and move things around.

The problem came when I was trying to hook the inspector window message loop – it didn’t seem to work. I fired up Spy++ and check that the inspector window was getting the messages – which it was – but they were never getting delivered to my hook code.

I was using System.Windows.Form.NativeWindow and overriding the WndProc function (see my NativeWindowListener class below). To get the hook in place I was creating a new NativeWindowListener class with the inspector window handle as the parameter. Checking the handles and the delegates etc it all seemed to be correct, I just couldn’t fathom why it wasn’t getting any of the messages.

Then the penny dropped. Outlook is in a separate process… (NativeWindow only works across the current process and window handles that are contained in it). What would be needed in this case is a system wide hook, that requires use of the Win32API and interop. I reckon this is overkill just to save a bit of development pain, so will probably move to something where the Outlook addin dynamically loads the panel in and make something available to have the add-in reload the panel – this should allow me to develop the panel code (the majority of the app) without worrying to much about the Outlook addin side of things.

using System;
using System.Windows.Forms;
using System.Text;

namespace Outlook\_Subclasser
{
\[System.Security.Permissions.PermissionSet(System.Security.Permissions.SecurityAction.Demand, Name = "FullTrust")\]
internal class NativeWindowListener : NativeWindow
{

    // Constant value was found in the "windows.h" header file.
    private const int WM\_SIZE = 0x0005;

    public NativeWindowListener(IntPtr handle)
    {
        AssignHandle(handle);
    }

    \[System.Security.Permissions.PermissionSet(System.Security.Permissions.SecurityAction.Demand, Name = "FullTrust")\]
    protected override void WndProc(ref Message msg)
    {
        switch (msg.Msg)
        {
            case WM\_SIZE:
                // do your processing in here
                break;

            default:
                break;
        }
        base.WndProc(ref msg);
    }
}
}

and then to use the class you would find the handle of the window you want to subclass and create a new NativeWindowsListener passing in that handle to the constructor. Note – this does not cover a number of aspects around releasing handles etc – you ***will*** want to make sure these are covered off…

MyForm myForm = new MyForm();
NativeWindowListener nwl = new NativeWindowListener(myForm.Handle);

GEO 51.4043197631836:\-1.28760504722595