---
title: "Tweet from PowerShell"
date: "2008-04-01T09:23:11"
tags: [
  "powershell"
]
---
[](https://kapie.com/content/binary/WindowsLiveWriter/TweetfromPowerShell_8A27/Windows_PowerShell_icon_2.png)[![twitter](twitter_thumb_1.gif)](https://kapie.com/content/binary/WindowsLiveWriter/TweetfromPowerShell_8A27/twitter_4.gif)I have been playing with [Twitter](http://twitter.com/) recently and thought it might be neat to see if I could post a ‘tweet’ from PowerShell. There is a great [Google Group that discusses their API](http://groups.google.com/group/twitter-development-talk/web/api-documentation). The APIs are all REST based and really easy to use – the only complexity is that you need HTTP Basic Authentication to do anything ‘real’.

One of the more simple API calls is to get the public timeline. No authentication is required for this so you can simply the url into your browser and get back the data (xml format, but json and other formats are available also). Try this :![Windows_PowerShell_icon](Windows_PowerShell_icon_thumb.png)

[http://twitter.com/statuses/public\_timeline.xml](http://twitter.com/statuses/public_timeline.xml)

Now, for doing an update we need the following API:

> ##### **update**
> 
> Updates the authenticating user’s status.  Requires the status parameter specified below.  Request must be a POST.
> 
> **URL:** http://twitter.com/statuses/update.*format*
> 
> **Formats:** xml, json.  Returns the posted status in requested format when successful.
> 
> **Parameters:**
> 
> -   status.  Required.  The text of your status update.  Be sure to URL encode as necessary.  Must not be more than 160 characters and should not be more than 140 characters to ensure optimal display.

The fact it must be a POST means we have to use a HttpWebRequest (as opposed to the easier WebClient). Anyway, here is the PowerShell function :

> > function Send-Tweet(\[string\]$text, \[string\]$username, \[string\]$password)
> > 
> > {
> > 
> >      $updateurl \= “[http://twitter.com/statuses/update.xml](http://twitter.com/statuses/update.xml)“
> > 
> >      $result \= $null
> > 
> >      $text \= \[System.Web.HttpUtility\]::UrlEncode($text)
> > 
> >      \[System.Net.HttpWebRequest\] $request \= \[System.Net.HttpWebRequest\] \[System.Net.WebRequest\]::Create($updateurl)
> > 
> >      $request.Credentials \= new-object System.Net.NetworkCredential($username, $password)
> > 
> >      $request.Method \= “POST”
> > 
> >      $request.ContentType \= “application/x-www-form-urlencoded”
> > 
> >      $param \= “status=” + $text
> > 
> >      $sourceParam \= “&source=PowerShell”
> > 
> >      $request.ContentLength \= $param.Length + $sourceParam.Length
> > 
> >      \[System.IO.StreamWriter\] $stOut \= new-object System.IO.StreamWriter($request.GetRequestStream(), \[System.Text.Encoding\]::ASCII)
> > 
> >      $stOut.Write($param)
> > 
> >      $stOut.Write($sourceParam)
> > 
> >      $stOut.Close()
> > 
> >      \[System.Net.HttpWebResponse\] $response \= \[System.Net.HttpWebResponse\] $request.GetResponse()
> > 
> >      if ($response.StatusCode \-ne 200)
> > 
> >      {
> > 
> >            $result \= “Error : “ + $response.StatusCode + ” : “ + $response.StatusDescription
> > 
> >      }
> > 
> >      else
> > 
> >      {
> > 
> >            $sr \= New-Object System.IO.StreamReader($response.GetResponseStream())
> > 
> >            \[xml\]$xml \= \[xml\]$sr.ReadToEnd()
> > 
> >            $id \= $xml.status.id
> > 
> >            $tweet \= $xml.status.text
> > 
> >            if ($tweet.length \-gt 50) { $tweet \= $tweet.Substring(0,50) + “…(truncacted)” }
> > 
> >            $result \= “Tweet “ + $id + ” added : “ + $tweet
> > 
> >      }
> > 
> >      return $result
> > 
> > }

And to use it :

> > send-tweet “I’m sending updates from PowerShell, cool or what ??” “<your\_username>” “<your\_password>”

GEO 51.4043197631836:\-1.28760504722595