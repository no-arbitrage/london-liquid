---
title: "PowerMeter from PowerShell"
date: "2010-10-01T00:05:15"
tags: [
  "powershell",
  "scripting"
]
---
[![image](image_thumb_1.png "image")](https://kapie.com/content/binary/WindowsLiveWriter/PowerMeterfromPowerShell_3F3/image_4.png) I was trying to get my [Home Energy Monitor](https://kapie.com/2010/09/27/CurrentCostPowerMonitorSoftwareTake2.aspx) application working to [Google PowerMeter](http://www.google.com/powermeter/about/index.html)[![image](image_thumb.png "image")](https://kapie.com/content/binary/WindowsLiveWriter/PowerMeterfromPowerShell_3F3/image_2.png) this evening. To get things moving quickly I decided to prototype in PowerShell (as you the full sugary goodness of the .Net framework for free). Here’s the details on accessing PowerMeter from PowerShell…

Although it is called [Google PowerMeter](http://www.google.com/powermeter/about/index.html), it is simply a service that records variables (of either cumulative or instantaneous values).

Firstly you need to get registered for an account, and it is not obvious how you actually get a [Google PowerMeter](http://www.google.com/powermeter/about/index.html) account if you don’t have some of the supported devices or don’t have a contract with one partner utility companies. The easy way is to put together a url that basically requests a new account. the format of the url is :-

[https://www.google.com/powermeter/device/activate?mfg=MMM&model=PPP&did=DDD&cvars=N](https://www.google.com/powermeter/device/activate?mfg=MMM&model=PPP&did=DDD&cvars=N)

All the details are on [this page](http://code.google.com/apis/powermeter/docs/powermeter_device_activation.html). I have a CurrentCost Envi, so my url became:

[https://www.google.com/powermeter/device/activate?mfg=CurrentCost&model=Envi&did=8097&dvars=1](https://www.google.com/powermeter/device/activate?mfg=CurrentCost&model=Envi&did=8097&dvars=1)

Note I’m using dvars at the end instead of cvar – dvars are for durational measurements and cvars are for instantaneous measurements – you need to get these right or your uploads will fail. the dvars=1 means I want only 1 variable (energy), I could have opted for more (dvars=2, or dvars=3 etc), but 1 will do for now.

[![image](image_thumb_3.png "image")](https://kapie.com/content/binary/WindowsLiveWriter/PowerMeterfromPowerShell_3F3/image_8.png)When you’ve created your url, simply browse to it. Google will authenticate you with your usual Google account and then ask you to give a friendly name to the variable(s) you created. When complete you’ll be presented with a activation code. You can get this activation again by browsing to your settings page in Google PowerMeter.  From this activation code you need 3 pieces of data as highlighted below :

[![image](image_thumb_4.png "image")](https://kapie.com/content/binary/WindowsLiveWriter/PowerMeterfromPowerShell_3F3/image_10.png)

The first is your ‘auth token’, the second is your ‘user id’ and the third is your ‘device id’.

Now for the PowerShell script. It is fairly simple, it creates an Xml string with the start date of the reading, the duration (in seconds) and the value and then uploads this to Google PowerMeter. It does need some Headers adding first to make sure your sending the correct Content-Type and to make sure you are authorized…

$auth = "YOUR AUTH TOKEN"
$user = "YOUR USER ID"
$device = "YOUR DEVICE ID"
$variable = "d1"
$url = "https://www.google.com/powermeter/feeds/event"
$var = "https://www.google.com/powermeter/feeds/user/$user/$user/variable/$device.$variable"

$start = \[System.Xml.XmlConvert\]::ToString(\[System.DateTime\]::Now)
$duration = 1
$energy = 9999

$xmlData = @"
    <feed xmlns=\`"http://www.w3.org/2005/Atom\`"
    xmlns:meter=\`"http://schemas.google.com/meter/2008\`"
    xmlns:batch=\`"http://schemas.google.com/gdata/batch\`">
    <entry>
        <category scheme=\`"http://schemas.google.com/g/2005#kind\`"
        term=\`"http://schemas.google.com/meter/2008#durMeasurement\`"/>
        <meter:subject>{0}</meter:subject>
        <batch:id>A.1</batch:id>
        <meter:startTime uncertainty=\`"1.0\`">{1}</meter:startTime>
        <meter:duration uncertainty=\`"1.0\`">{2}</meter:duration>
        <meter:quantity uncertainty=\`"0.001\`" unit=\`"kW h\`">{3}</meter:quantity>
    </entry>
    </feed>
"@

$rdgData = \[string\]::Format($xmlData, $var, $start, $duration, $energy)

$wc = New-Object System.Net.WebClient
$whc = New-Object System.Net.WebHeaderCollection$res
$whc.Add("Content-Type", "application/atom+xml")
$whc.Add("Authorization", "AuthSub token=\`"$auth\`"")
$wc.Headers = $whc

$response = $wc.UploadString($url, "POST", $rdgData)

Now you have the xml response in the **$response** variable. To check this you can simply **(\[xml\]$response).feed.entry.status.code** – you’re looking for a 201 (‘Created’).  
You should now have a measurement lodged with [Google PowerMeter](http://www.google.com/powermeter/about/index.html) !!  Enjoy…

GEO 51.4043388366699:\-1.2875679731369