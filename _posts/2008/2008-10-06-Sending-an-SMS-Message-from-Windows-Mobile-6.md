---
title: "Sending an SMS Message from Windows Mobile 6"
date: "2008-10-06T22:37:41"
tags: [
  "technical"
]
---
[![tytnII_101x111_thumb](tytnII_101x111_thumb_thumb.jpg)](https://kapie.com/content/binary/WindowsLiveWriter/SendinganSMSMessagefromWindowsMobile6_14C42/tytnII_101x111_thumb_2.jpg)Programmatically sending an SMS message from your Windows Mobile is fairly simple these days.

You’ll need the WM6 SDK. Start a new Visual Studio ‘Smart Device Project’, add a reference to Microsoft.Windows.PocketOutlook, then use the following code…

using Microsoft.WindowsMobile.PocketOutlook;

..and..

public void Send(string to, string msg)
{
    if ((to != string.Empty) && (msg != string.Empty))
    {
        SmsMessage message = new SmsMessage(to, msg);
        try
        {
            message.Send();
        }
        catch (Exception ex)
        {
            MessageBox.Show(ex.ToString());
        }
    }
}

Job done…

GEO 51.4043197631836:\-1.28760504722595