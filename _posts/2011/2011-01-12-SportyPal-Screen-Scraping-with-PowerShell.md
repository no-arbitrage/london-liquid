---
title: "SportyPal Screen Scraping with PowerShell"
date: "2011-01-12T11:42:37"
tags: [
  "powershell",
  "web"
]
---
[![image](image_thumb.png "image")](https://kapie.com/content/binary/Windows-Live-Writer/SportyPal-Screen-Scraping-with-PowerShel_F791/image_2.png)For the past year (2010) I had been using [SportyPal](http://www.sportypal.com/) – an application for tracking exercise (runs mainly for me). It has mobile apps (iPhone, Android, WinMob etc.) that do the actual tracking and then upload the data to their website where you can view history, graphs, charts, records etc. It really is neat, and the user interface on the mobile device really looks good (at least the WinMob and Android versions I used)…

I have been pinging them on their forums for a while now about when their subscription service would be launched and about getting a subscription trial – [they announced that they expected it in Summer 2010](http://www.sportypal.com/sportypal2), it got delayed and delayed and although I got vague answers about release dates from their forum and twitter responses, it still was not available by the end of December 2010.

[![image](image_thumb_1.png "image")](https://kapie.com/content/binary/Windows-Live-Writer/SportyPal-Screen-Scraping-with-PowerShel_F791/image_4.png)So, much as I loved their app, it was time to switch – [RunKeeper](http://runkeeper.com) was the new app/service I chose. The problem I faced was how to get my years worth of data out of SportyPal – they do allow you to export GPX data for each run, but it’s on a run by run, manual basis – not good.

Time to crack open PowerShell…

A bit of poking around with Chrome Developer Tools and [Fiddler2](http://www.fiddler2.com/fiddler2/) identified the sequence for logging in and the format of the history/activity page that lists all runs.  
So now I had a basis for screen-scraping the data I needed, and I also notice that for each run there was a link to download the GPX.

I put together a script that would login, open the activities page and grab the data about each run, it also downloads a copy of the GPS data to a separate file for each run, with the filename set as the date/time stamp of the run.  
I couldn’t see any easy method of importing (but to be honest I didn’t look for very long), but as I only had 100 runs to import I simply did it manually (RunKeepers import function is only about 3 clicks).

Initially I found that every GPX file I imported came up with an error about the GPX being invalid, however, after a browse around the forums I found that one of the namespaces was incorrect (http://www.topografix.com/GPX/1/0 when it should be http://www.topografix.com/GPX/1/1). This did the trick and now on importing each GPX file the correct run details, route etc all showed up – so all was good.

Here’s the PowerShell script – it logs in to your account, screen scrapes all your activities and then downloads the GPX for each. It also ‘fixes’ the downloaded GPX so it has the correct namespace :

$email = "your\_email"
$password = "your\_password"


$url = "http://www.sportypal.com/Login"

"Starting..."
""

     \[System.Net.HttpWebRequest\] $request = \[System.Net.HttpWebRequest\] \[System.Net.WebRequest\]::Create($url)
     $cookieJar = new-object "System.Net.CookieContainer"
     $request.CookieContainer = $cookieJar
     $request.Method = "POST"
     $request.ContentType = "application/x-www-form-urlencoded"
     $param = "Email=" + $email + "&Password=" + $password + "&login=Login"

     $request.ContentLength = $param.Length

     \[System.IO.StreamWriter\] $stOut = new-object System.IO.StreamWriter($request.GetRequestStream(), \[System.Text.Encoding\]::ASCII)
     $stOut.Write($param)
     $stOut.Write($sourceParam)
     $stOut.Close()

     "Logging in..."
     ""

     \[System.Net.HttpWebResponse\] $response = \[System.Net.HttpWebResponse\] $request.GetResponse()

     if ($response.StatusCode -ne 200)
     {
           $result = "Error : " + $response.StatusCode + " : " + $response.StatusDescription
     }
     else
     {
           $sr = New-Object System.IO.StreamReader($response.GetResponseStream())
           $txt = $sr.ReadToEnd()
           $cutstart = $txt.Substring($txt.IndexOf('<table id="my\_workouts"'))
           $cutend = $cutstart.Substring(0,$cutstart.IndexOf("</div>"))

           "Getting workouts"
           $workouts = @()
           $ipos = 0
           while(($ipos -ne -1) -and ($ipos -lt ($cutend.Length -1)))
           {
                $s = $cutend.IndexOf("<tr id=", $ipos)
                if ($s -ne -1)
                {
                    $e = $cutend.IndexOf("</tr>", $s)
                }
                else
                {
                    $e = -1
                }
                if(($e -ne -1) -and ($s -ne -1))
                {
                    $tr = $cutend.Substring($s, ($e + 5) - $s)
                    $workouts += $tr
                }
                $ipos = $e
           }

           #$workouts | %{ $id = $\_.Substring(11,6); $id }

           "Got " + $workouts.Length + " workouts"
           foreach($wo in $workouts)
           {
                $id = $wo.Substring(11,6)
                $s = $wo.IndexOf("dateval\_$id") + 23
                $dt = (New-Object "System.dateTime"(1970,1,1)).AddMilliseconds($wo.Substring($s,13))
                $s = $wo.IndexOf("td\_number clickDetails") + 24
                $e = $wo.IndexOf("</td>", $s)
                $dist = $wo.Substring($s, $e-$s).Trim()
                $dist = $dist.Substring(0, $dist.Length-2)
                $s = $wo.IndexOf("td\_number clickDetails", $e) + 24
                $e = $wo.IndexOf("</td>", $s)
                $time = $wo.Substring($s, $e-$s).Trim()
                $s = $wo.IndexOf("td\_number clickDetails", $e) + 24
                $e = $wo.IndexOf("</td>", $s)
                $cals = $wo.Substring($s, $e-$s).Trim()
                $cals = $cals.Substring(0, $cals.Length-4)
                "Workout on $dt ( ID = $id ) : $dist : $time : $cals calories"

                # now grab the GPX
                $filename = "c:scriptssportypal" + $dt.ToString("yyyy-MM-dd\_HHmm") + ".gpx"
                $gpxUrl = "http://www.sportypal.com/Workouts/ExportGPX?workout\_id=$id"
                \[System.Net.HttpWebRequest\] $gpxRequest = \[System.Net.HttpWebRequest\] \[System.Net.WebRequest\]::Create($gpxUrl)
                $gpxRequest.CookieContainer = $request.CookieContainer
                $gpxRequest.AllowWriteStreamBuffering = $false
                $gpxResponse = \[System.Net.HttpWebResponse\]$gpxRequest.GetResponse()
                \[System.IO.Stream\]$st = $gpxResponse.GetResponseStream()

                # write to disk
                $mode = \[System.IO.FileMode\]::Create
                $fs = New-Object System.IO.FileStream $filename, $mode
                $read = New-Object byte\[\] 256
                \[int\] $count = $st.Read($read, 0, $read.Length)
                while ($count -gt 0)
                {
                    $fs.Write($read, 0, $count)
                    $count = $st.Read($read, 0, $read.Length)
                }
                $fs.Close()
                $st.Close()
                $gpxResponse.Close()
                "- GPX Data in $filename"
           }

     }

     $response.Close()



    # now fix the namespace error
    $files = gci "c:scriptssportypal\*.\*"
    foreach ($file in $files)
    {
        "Fixing namespace error in " + $file
        $content = get-content $file
        $content\[0\] = $content\[0\].Replace('xmlns="http://www.topografix.com/GPX/1/0"', 'xmlns="http://www.topografix.com/GPX/1/1"')
        Set-Content $file $content
    }

    "Complete"

Feel free to use this (at your own risk – I accept no liability whatsoever), but realise that you may be breaking all manner of T’s & C’s for SportyPal.

… and on a final note to anyone from SportyPal :

“*Sorry. I persevered, I really did, I was ready to throw money at you for a PRO subscription, I could even have lived with a further slipped release date, but not keeping your fans updated, giving no idea of a roadmap – doesn’t make us feel the love*”.

GEO: 51.4043807983398 : \-1.2872029542923

**UPDATE**: Thanks to Ricardo, who identified a bug where workouts marked as private would not download correctly. The script is now updated with this fix.