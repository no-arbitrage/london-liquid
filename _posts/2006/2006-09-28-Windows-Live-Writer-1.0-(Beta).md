---
title: "Windows Live Writer 1.0 (Beta)"
date: "2006-09-28T09:30:11"
tags: [
  "development",
  "technical",
  "tools"
]
---
Released today, a new version – [get it here](http://g.msn.com/8SEENUS030000TBR/WriterMSI). They have added the following (list extracted from the [Windows Liver Writer blog post](http://windowslivewriter.spaces.live.com/blog/cns!D85741BB5E0BE8AA!702.trak))

*The following is a summary of the changes in the Writer 1.0 (Beta) Update:*

-   *Tagging support*
-   *Support for Blogger Beta*
-   *Categories are sorted by name and support scrolling, plus improved support for reading categories from your blog*
-   *Improved startup performance*
-   *Paste is enabled for Title region and TAB/SHIFT+TAB navigation between title and body supported*
-   *Insert hyperlink added to context menu when text is selected*
-   *Title attribute in Insert Link dialog*
-   *Custom date support for Community Server*
-   *Improved keyboard shortcuts for switching views*
-   *Change spell-check shortcut key to F7*
-   *Add ‘png’ to insert image dialog file type filter*
-   *More robust image posting to Live Spaces*
-   *Improved style detection for blogs* 
-   *Fixed issues with pasting URLs and links*
-   *Remember last window size and position when opening a new post*
-   *Open post dialog retrieves more than 25 old posts*

From what I have noticed :-

-   It still takes ***an age*** to open.
-   There is still no support for adding a file to a post (and having it uploaded / FTP’d), so my [‘Insert File (via FTP) plugin’](https://kapie.com/2006/09/21/WindowsLive+Writer+Plugin.aspx) is still valid.
-   There is still no support to add new categories (you can choose from existing categories)
-   The insert ‘task box’ does not provide a scrollbar[![](Capture_09-28-2006_1_thumb%5B7%5D.png)](https://kapie.com/content/binary/WindowsLiveWriter/WindowsLiveWriter1.0Beta_8CD3/Capture_09-28-2006_1%5B9%5D.png) (so some items get hidden as you create more drafts)

Other things I noticed about the SDK when writing my [plugin](https://kapie.com/2006/09/21/WindowsLive+Writer+Plugin.aspx) :

-   No way to get a ref to the current blog provider
-   No way to get properties for the current blog (this would be great as my plugin would be able to automatically pick up the FTP settings)
-   The icon size for displaying on the Insert tab is a crazy 20×18. Why didn’t they simply go with a standard size (16×16 for example).

> *See ‘Insert File (via FTP) icon on the embedded image for how it looks when trying to scale a standard image to this custom size – AWFUL.*

However, I can’t complain, overall I’m pretty happy – it’s **FREE**, it makes my post creating **MUCH FASTER** and the way it shows me how the post will look on the website (automagically using my website CSS / formatting) is just **AMAZING**