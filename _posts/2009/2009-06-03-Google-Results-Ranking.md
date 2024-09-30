---
title: "Google Results Ranking"
date: "2009-06-03T22:43:37"
tags: [
  "powershell",
  "scripting",
  "web"
]
---
***Disclaimer**: Screenscraping results like this probably contravening Google’s Terms of Use (or something) and I do not advocate that you do it – this is purely hypothetical, if I did want to do it, this is how I would go about it  😉*

***Further Disclaimer:** The results page formats could change at any time and may well break this script, if that happens you are on your own (FireBug and some modified regex should help you out).*

[![image](image_thumb.png "image")](https://kapie.com/content/binary/WindowsLiveWriter/GoogleResultsRanking_14D92/image_2.png)

So, if you wanted to get the Google ranking of a bunch of domains when searching for a particular term you could use one of the many SEO page ranking test sites that are available, but these are a pain in as much it they require you to enter the search term and the domain name you are looking for and they give you the ranking (what position in the results the domain name comes). that is fine for individual searches (like what position is kapie.com if I search on ‘Ken Hughes’), but not very good for doing a comparison of multiple domains against the search term.

I looked at using Googles Search API to get this info, but unfortunately it only returns 4 or 8 results (it is mainly designed to present some brief results in a box on your website), what I needed was to look at a lot more results (like up to 500)….

Back to my trusty friend – PowerShell…

I create a web client, have it download the first X (500) results to the search term, load the link Url and the position into a hashtable and then lookup the hashtable to find the rank position of each of the domain names I am looking for.  
It was actually pretty easy, the only difficult part was getting the regex(s) correct – Regex is evil, as evil as Perl….

Here is the script code :

  $domainNames = "google.com", "live.com", "bing.com", "yahoo.com"
  $maxResult = 100
  $searchTerm = "search"

  $urlPattern = "<s\*as\*\[^>\]\*?hrefs\*=s\*\[\`"'\]\*(\[^\`"'>\]+)\[^>\]\*?>"
  $hitPattern = "<s\*(h3)sclass=r>(.\*?)</1>"

  $wc = new-object "System.Net.WebClient"
  $urlRegex = New-Object System.Text.RegularExpressions.Regex $urlPattern
  $hitRegex = New-Object System.Text.RegularExpressions.Regex $hitPattern
  $urls = @{}

  $resultsIndex = 0
  $count = 1
  while($resultsIndex -lt $maxResults)
  {
    $inputText = $wc.DownloadString("http://www.google.com/search?q=$searchTerm&start=$resultsIndex")

    "Parsing : " + $resultsIndex

    $index = 0
    while($index -lt $inputText.Length)
    {
      $match = $hitRegex.Match($inputText, $index)
      if($match.Success -and $match.Length -gt 0)
      {
        $urlMatch = $urlRegex.Match($match.Value.ToString())
        if(($urlMatch.Success) -and ($urlMatch.Length -gt 0))
        {
          $newKey = $urlMatch.Groups\[1\].Value.ToString()
          if(!$urls.ContainsKey($newKey))
          {
            $urls.Add($newkey, $count)
          }
          $count++
        }
        $index = $match.Index + $match.Length
      }
      else
      {
        $index = $inputText.Length
      }
    }
    $resultsIndex += 10
  }


  foreach($domain in $domainNames)
  {
    $maxPos = -1
    foreach($key in $urls.Keys)
    {
      if($key.Contains($domain))
      {
        $pos = \[int\] $urls\[$key\]
        if(($pos -lt $maxPos) -or ($maxPos = -1))
        {
          $maxPos = $pos
        }
      }
    }
    if($maxPos -eq -1)
    {
      $domain + " : Not Found"
    }
    else
    {
      $domain + " : Found at result #" + $maxPos
    }
  }

Drop me a line in the comments if you find it useful…

GEO 51.4043197631836:\-1.28760504722595