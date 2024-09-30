---
title: "IHttpModule redirector"
date: "2007-03-08T20:10:19"
tags: [
  "dasblog",
  "development"
]
---
 I use dasBlog as the vehicle/technology for this site and currently have it configured at the root of the website (so you go to [https://kapie.com](https://kapie.com) and you get directly into this blog). This is kind of an unusual setup as most people put it in a /blog subdirectory or virtual directory.

I have plans to put a fair bit more static content up so I wanted to change to the standard of /blog subfolder – the problem is that all my current links will fail when I move the content along with all the content google has cached / indexed about the site – Not Good.

[Scott Hanselman](http://www.hanselman.com) suggested in response to a question I posed on the dasBlog mailing list that writing a HttpModule with a HTTP temporary redirect (HTTP response code 302) might be the way to go and reckoned it might only be a handful of lines of code….

 I was pleasantly surprised at how easy it was…

I have included the code below in case anyone else might find it useful. It is pretty specific at the moment (takes the requested URL and if it does not include /blog/ then it insert it) but it could easily by made generic (RegEx or the like).

Happy to share the actual source files if anyone wants them – drop me a mail/comment. 

using System;

using System.Web;

using System.Text;

namespace KH

{

    public class Redirector : IHttpModule

    {

        private EventHandler onBeginRequest;

        public Redirector()

        {

            onBeginRequest = new EventHandler(this.HandleBeginRequest);

        }

        void IHttpModule.Dispose() { }

        void IHttpModule.Init(HttpApplication context)

        {

            context.BeginRequest += onBeginRequest;

        }

        private void HandleBeginRequest(object sender, EventArgs evargs)

        {

            HttpApplication app = sender as HttpApplication;

            if (app != null)

            {

                string host = app.Request.Url.Host;

                string requestUrl = app.Request.Url.PathAndQuery;

                if (requestUrl.IndexOf(“/blog/”) == -1)

                {

                    Uri newURL = new Uri(app.Request.Url.Scheme + “://www.” + host + “/blog” + requestUrl);

                    app.Context.Response.RedirectLocation = newURL.ToString();

                    app.Context.Response.StatusCode = 302;

                    app.Context.Response.End();

                    return;

                }

                else

                {

                    return;

                }

            }

        }

    }

}