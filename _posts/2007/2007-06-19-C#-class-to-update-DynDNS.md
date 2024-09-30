---
title: "C# class to update DynDNS"
date: "2007-06-19T01:27:49"
tags: [
  "development",
  "tools"
]
---
I’m in Western Mass (Westboro, MA) again this week and while that normally means 16 hour days (there is nothing much else to do but sit around a hotel room so why not…), this week I decided to work on a personal project for a bit.

Part of it was some code to check a machines public IP address and update it to [DynDNS](http://www.dyndns.com/). Luckily they have an excellent [developers / API section](http://www.dyndns.com/developers/specs/flowdiagram.html) that explains everything you need to do this. There are two sections to it, detecting the machines public IP address and updating the hostnames associated with your account, both of which they provide a service for.

It’s pretty easy to follow and within an hour or so I had come up with a class to do both tasks. Here is a snippet for the ‘CheckIP’ function:

public static string CheckIP()

{

    // check the current public IP address

    string ipString = string.Empty;

    WebClient wc = new WebClient();

    try

    {

        string result = wc.DownloadString(“http://checkip.dyndns.com”);

        return ParseCheckResult(result);

    }

    catch (WebException)

    {

        ipString = string.Empty;

    }

    return ipString;

}

Just call a URL ([http://checkip.dyndns.com](http://checkip.dyndns.com)) and it returns your public IP address, there was some parsing of the return text but it is pretty simple. Click the link and see for yourself.

Next was the ‘UpdateIP’ function, here’s the snippet:

public static string UpdateIP(string username, string password, string ipaddr, string hosts)

{

    // check the current public IP address against what we want to update to

    string updateurl = “http://members.dyndns.org/nic/update?hostname”;

    string result = string.Empty;

    WebClient wc = new WebClient();

    string url = string.Format(@”{0}={1}&myip={2}&wildcard=NOCHG&mx=NOCHG&backmx=NOCHG”, updateurl, hosts, ipaddr);

    try

    {

        wc.Credentials = new NetworkCredential(username, password);

        wc.Headers.Add(“User-Agent”, “KSL – WHS Updater – 1.0”);

It is basically just a case of passing a querystring with all the details to [http://members.dyndns.org/nic/update](http://members.dyndns.org/nic/update) , ensuring you set the credentials to your [DynDNS](http://www.dyndns.com/) account username and password and specifying a unique User-Agent.

[The complete project files (including a small ‘user’ application to test it) can be found here (61K).](https://kapie.com/content/binary/WHS%20DynDNS%20Addin.zip "DynDNS Project Code")