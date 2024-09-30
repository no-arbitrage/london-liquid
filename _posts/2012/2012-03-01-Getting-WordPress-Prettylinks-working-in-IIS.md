---
title: "Getting WordPress Prettylinks working in IIS"
date: "2012-03-01T09:00:30"
tags: [
  "web"
]
---
![](75px-Wordpress-logo-simple.png)Today, I was restoring a backup of a WordPress site I have. Copied all the files into a folder, squirted all the data back into MySQL and set up a separate website in IIS. All was fine, I could browse to the main page [www.domain.com/index.php](http://www.domain.com/index.php) and got the page I expected, but clicking on any of the links gave me a 404 error. A bit of checking uncovered that it was probably down to the pretty links that WordPress was configured to use.

It seems that IIS interprets [http://www.domain.com/category/blog-post-title/](http://www.domain.com/category/blog-post-title/) as a request to view the folder contents or the default page in that particular folder ( /category/blog-post-title ), when in actual fact WordPress is expecting that URL to be passed to index.php so that it can be parsed and the correct content rendered.

Anyway, this can be solved via a simple IIS Rewrite Rule. To get this working add the following to the web.config file that is in the same folder as all the WordPress files :-

<rewrite>
    <rules>
        <rule name="Main Rule" stopProcessing="true">
            <match url=".\*" />
            <conditions logicalGrouping="MatchAll">
                <add input="{REQUEST\_FILENAME}" matchType="IsFile" negate="true" />
                <add input="{REQUEST\_FILENAME}" matchType="IsDirectory" negate="true" />
            </conditions>
            <action type="Rewrite" url="index.php" />
        </rule>
    </rules>
</rewrite>

This needs to into the <system.webServer> section.