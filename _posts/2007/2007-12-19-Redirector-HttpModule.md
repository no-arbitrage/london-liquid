---
title: "Redirector HttpModule"
date: "2007-12-19T19:09:38"
tags: [
  "development",
  "web"
]
---
[![image](image_thumb.png)](https://kapie.com/content/binary/WindowsLiveWriter/RedirectorHttpModule_DAD0/image_2.png)Originally I had my website set up to default to my blog (so dasBlog was installed at the website root level instead of in a /blog subfolder).

Recently I have been writing some Web Applications and wanted to restructure my website so that there is a subfolder for each app / main area (blog, projects, etc).

The problem being that all the links out there point to [https://kapie.com/2007/12/19/RedirectorHttpModule.aspx](https://kapie.com/2007/12/19/RedirectorHttpModule.aspx) and these would all be dead link when moved to [https://kapie.com/2007/12/19/RedirectorHttpModule.aspx](https://kapie.com/2007/12/19/RedirectorHttpModule.aspx), so I needed to respond with a HTTP 302 to the original links and tell the client that it should temporarily redirect to the new location (changing to a HTTP 301 when I am happy that it is all working).

This called for a HttpModule…

namespace KSL
{
     public class BlogRedirector : IHttpModule
     {
[![VisualStudioLogo](VisualStudioLogo_thumb.png)](https://kapie.com/content/binary/WindowsLiveWriter/RedirectorHttpModule_DAD0/VisualStudioLogo.png)         private EventHandler onBeginRequest;

public BlogRedirector()  
{  
onBeginRequest = new EventHandler(this.HandleBeginRequest);  
}

void IHttpModule.Dispose()  
{  
}

void IHttpModule.Init(HttpApplication context)  
{  
context.BeginRequest += onBeginRequest;  
}

private void HandleBeginRequest( object sender, EventArgs evargs )  
{  
HttpApplication app = sender as HttpApplication;

if ( app != null )  
{  
HttpContext context = app.Context;  
string host = app.Request.Url.Host;  
string requestUrl = app.Request.Url.PathAndQuery;

if (requestUrl.Contains(@”/blog/”)  
|| requestUrl.Contains(@”/foo1/”)  
|| requestUrl.Contains(@”/foo2/”)  
|| requestUrl.Contains(@”/foo3/”)  
|| requestUrl.Contains(@”/foo4/”)  
|| (requestUrl.ToLower() == app.Request.Url.Scheme + @”://” + host + @”/default.aspx”))  
{  
return;  
}  
else  
{  
Uri newURL = new Uri(app.Request.Url.Scheme + @”://” + host + @”/blog” + requestUrl);  
context.Response.RedirectLocation = newURL.ToString();  
context.Response.StatusCode = 302;  
context.Response.End();  
return;  
}  
}  
}  
}  
}

This basically allows me to redirect any url that does not contain **/blog/** or **/foo\*/** or is not **/default.aspx** by sending a 302 status code and adding **/blog/** to the url to generate the redirect url. The compiled DLLs were stored in the bin folder off the root and the web.config in the root needed the following adding…

<?xml version\="1.0" encoding\="UTF-8"?>
<configuration\>
  <system.web\>
    <httpModules\>
      <add type\="KSL.BlogRedirector,BlogRedirector" name\="BlogRedirector" />
    </httpModules\>
  </system.web\>
</configuration\>

This seemed to work fine some of the time, but was a bit hit and miss…. Further investigation pointed me at the web.config files in the subfolders and that they would inherit the sections from the root web.config. So all the subfolder web.config files got the following added.

<?xml version\="1.0" encoding\="UTF-8"?>
<configuration\>
  <system.web\>
    <httpModules\>
      <remove name\="BlogRedirector" />
    </httpModules\>
  </system.web\>
</configuration\>

GEO 51.4043197631836:\-1.28760504722595