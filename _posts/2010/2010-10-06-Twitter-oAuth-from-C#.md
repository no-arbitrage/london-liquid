---
title: "Twitter oAuth from C#"
date: "2010-10-06T11:44:10"
tags: [
  "development",
  "web"
]
---
A while back I was working on a ‘post to Twitter’ function that used the original Basic Authentication that the Twitter V1.0 API allowed.

Unfortunately, at the end of Aug 2010 they discontinued support for this and forced everyone to use their oAuth authentication. There are a number of services around (such as [SuperTweet](http://www.supertweet.net/)) that will proxy your Basic Auth to Twitters oAuth, but really your best bet is to upgrade your code to support oAuth.

It is actually not as difficult as it may sound – here’s how I upgraded my code :-

First go to [http://dev.twitter.com/apps/new](http://dev.twitter.com/apps/new) and register your application. When complete you’ll get the details you need in a page like this :

[![image](image_thumb.png "image")](https://kapie.com/content/binary/WindowsLiveWriter/7c2ba9ea1522_FC77/image_2.png)

You do need to make sure that you have enabled both Read and Write access if you plan to post updates from your application.

Next, grab this [zip file](https://kapie.com/files/oAuthTwitter.zip) with the oAuth.cs and oAuthTwitter.cs classes that [Eran Sandler](http://eran.sandler.co.il/) wrote and [Shannon Whitley](http://www.voiceoftech.com/swhitley/index.php/category/software-development/page/2/) extended, and add those to your project (make sure you change the namespace !)

Now in your class you need to create an oAuth object and assign the ConsumerKey and ConsumerSecret properties to the values given to you in the Twitter API settings page. Now when someone wants to Authorize your app to work with their account you need to get the authorization link and fire up a browser navigating to that link :

oAuthTwitter \_oAuth = new oAuthTwitter();

\_oAuth.ConsumerKey = "AAAAAAAAVGJTAZerhSFsafvg";
\_oAuth.ConsumerSecret = "AAAAAAAgSgLfwFZDSQ3AZNDA5LwMfPnmPJud7kbCzo";

string authLink = \_oAuth.AuthorizationLinkGet();
System.Diagnostics.Process.Start(authLink);

The result of them clicking ‘Allow’ in the web browser is a PIN. You need to use that PIN in a call to AccessTokenGet which finally populates the Token and TokenSecret properties of the oAuth object.

\_oAuth.AccessTokenGet(\_oAuth.OAuthToken, thePin);
string Token = \_oAuth.Token;
string TokenSecret = \_oAuth.TokenSecret;

Now (very important) you need to save the Token and TokenSecret for later use (you don’t want your user having to authorize for every update).  
To send an update is now a simple affair – create an oAuthTwitter object, set the properties and then use the oAuthWebRequest function:

oAuthTwitter \_oAuth = new oAuthTwitter();

\_oAuth.ConsumerKey = "12345GJITAZerhSFsafvg";
\_oAuth.ConsumerSecret = "lkjghKJGHblgLfwFZDSQ3AZNDA5RLwMfPnmPJud7kbCzo";
\_oAuth.PIN = thePin;
\_oAuth.Token = theToken;
\_oAuth.TokenSecret = theTokenSecret;

string url = "http://twitter.com/statuses/update.xml";
string tweet = System.Web.HttpUtility.UrlEncode("status=" + "Tweet from my application!");
string xml = \_oAuth.oAuthWebRequest(oAuthTwitter.Method.POST, url, tweet);

We’re done ! Less than 20 lines of code.

GEO: 51.4043862206013 : \-1.28720283508302