---
title: "CurrentCost Power Monitor Software"
date: "2010-05-11T16:13:16"
tags: [
  "development",
  "technical"
]
---
[![cc128-large_01](cc128-large_01_thumb.jpg "cc128-large_01")](https://kapie.com/content/binary/WindowsLiveWriter/CurrentCostPowerMonitorSoftware_BF4D/cc128-large_01_2.jpg) I recently bought a device to monitor our household energy consumption. After looking at a few I lumped for a [CurrentCost Envi](http://currentcost.com/software-downloads.html). this is a great little device that comes in two parts – a transmitter with jaws that wrap around the main power cable coming into your home and a desktop display unit. The communication is all wireless and I have found that it works okay through two thick brick walls.

the reason for going with this particular unit was that it has a data port that can connect to a USB port and feed it’s readings to a PC – I wanted to be able to monitor the readings on my PC and chart/analyse them at will.

[![image](image_thumb_1.png "image")](https://kapie.com/content/binary/WindowsLiveWriter/CurrentCostPowerMonitorSoftware_BF4D/image_4.png)The communication to the PC is basically via USB, but emulating a serial port COM connection – the [CurrentCost](http://currentcost.com/) website has links to the [drivers](http://currentcost.com/software-downloads.html) you’ll need for this.

I wanted to, not only, record the readings but to chart them (on my website), get regular updates via various notification methods and a few other bits. Unfortunately, none of the [software applications](http://currentcost.com/software-downloads.html) listed on their website covered all the items that I wanted – so I set about writing something myself…

My initial thoughts were around just sending the data to Google PowerMeter, but at that stage the API was not public and their forums/groups were not very active (6 posts in about 18 months), so I abandoned that and decided a ‘DIY’ approach was needed.  
*UPDATE: It looks like there may be some substance to it now, so that is another area to look at (another plugin)…*

[![image](image_thumb.png "image")](https://kapie.com/content/binary/WindowsLiveWriter/CurrentCostPowerMonitorSoftware_BF4D/image_2.png)The requirements were pretty simple – I wanted a service that grabbed the readings as they came in, decoded them into something intelligible and then pushed them out to a number of ‘modules’ that would do something with the reading. the ‘modules’ would be self contained and new modules could be added at any time. The initial modules would be :-

-   Tweet the reading at a regular interval
-   Record the reading (in a database, Xml file or the like)
-   Send the reading to a website / webservice

It seemed a simple Windows Service with a COM port would be enough to grab the reading, the readings are all in Xml so another class to parse the Xml would be needed and then for the ‘modules’ a ‘plugin’ type architecture was called for. I came up with an interface that all plugins would implement and a method of loading them in dynamically.

Each plugin would inherit from the IPowerMonitorPlugin interface and to load the plugins, each one would be specified in the app.config file with a filename and classname. The service would look at each plugin entry, load the DLL and create an instance of the plugin class :-

    private void LoadPlugins()
    {
        plugins = new ArrayList();

        NameValueCollection appSettings = ConfigurationManager.AppSettings;
        foreach (string key in appSettings.AllKeys)
        {
            if(key.ToLower().StartsWith("plugin"))
            {
                string path = AppDomain.CurrentDomain.BaseDirectory;
                string\[\] config = appSettings.Get(key).Split(':');
                Assembly ass = Assembly.LoadFile(path + config\[0\]);
                IPowerMonitorPlugin plugin = (IPowerMonitorPlugin)ass.CreateInstance(config\[1\]);
                if (null == plugins)
                {
                    plugins = new ArrayList();
                }
                if (!plugins.Contains(plugin))
                {
                    plugin.Init();
                    plugins.Add(plugin);
                }

            }
        }
    }

When the service was working and decoding the readings correctly, I started adding plugins – first was a simple ‘RawXmlWriterPlugin’ – this simple wrote the raw Xml reading data (Reading.RawXml) out to a text file – just to make sure it was working and we were decoding the Xml correctly.

[![image](image_thumb_3.png "image")](https://kapie.com/content/binary/WindowsLiveWriter/CurrentCostPowerMonitorSoftware_BF4D/image_8.png)The next service was posting the data to a website – I found this [great website (pachube.com)](http://www.pachube.com/) which allows you to track environmental data measurements, have multiple feeds, multiple measurements and has a lot of options for getting data in and out. The API that they provide is pretty simple to push data into and their website allows plenty of ways to visualize the data – for example here is a [link to my last 24 hours energy consumption in a chart format](http://www.pachube.com/feeds/7255/datastreams/0/history.png?w=600&h=300&c=33cc66&b=true&g=true&t=Temperature&l=&s=6&r=2), [here is my latest temperature reading](http://www.pachube.com/feeds/7255/datastreams/1) and here is an [archive of all the energy data I have ever posted](http://www.pachube.com/feeds/7255/datastreams/0/archive.csv). there are also mash up to things like iGoogle widgets, Google’s Visualization API, Google Sketchup, iPhone Apps, Android Apps and this [rather neat gauge](http://apps.pachube.com/scaredycat/gauge.swf?xml_source=http://apps.pachube.com/scaredycat/getData.php%3Fm%3D0%26f%3D7255%26s%3D0%26u%3D8000%26l%3D100%26n%3D5%26t%3DEnergy%20Usage%26w%3Dtrue%26c1%3D33FF33%26c2%3DEFE415%26c3%3DEF8B15%26c4%3DFF3333%26in%3Dfalse).

From there I started the Twitter service….

**[![image](image_thumb_2.png "image")](https://kapie.com/content/binary/WindowsLiveWriter/CurrentCostPowerMonitorSoftware_BF4D/image_6.png)****PachubePosterPlugin**

The plugin for posting to Pachube requires that you have already create a feed with two datastreams (datastream 0 is energy and datastream 1 is temperature). You will need an API Key and the feed id. Both these items are configured in the app.config file also.

**TwitterPlugin**

The plugin for ‘tweeting’ to twitter is also pretty simple – all you need is an account (username and password) and the ‘handle’ of the person/account you want to send the message to (if you want to send a direct message). You also specify the message text you wish to send (with placeholders for the energy and temperature values) an the interval (in minutes) of how frequently you wish to send the tweets.

**SQL / Xml Storage Plugin**

Watch this space … 😉

**Source and Package**

For the time being either send me an email or post in the comments if you’d like access to the source code.

If you want to run this software yourself then here is a [link to a zip file containing the full package](https://kapie.com/files/powermonitorproject.zip). To get it installed, do the following :

-   [![image](image_thumb.png "image")](https://kapie.com/content/binary/WindowsLiveWriter/CurrentCostPowerMonitorSoftware_12A5E/image_2.png)Unzip the package contents into a folder (“C:Program FilesPowerMonitorService” would be good)
-   Open a command prompt and change to the folder above
-   Run the following command line…

> **“C:WindowsMicrosoft.NETFramework64v2.0.50727InstallUtil.exe” PowerMonitorService.exe** (if you are running a 64 bit machine… or)  
> **“C:WindowsMicrosoft.NETFrameworkv2.0.50727InstallUtil.exe” PowerMonitorService.exe** (if you are running a 32 bit machine… or)

-   Open the PowerMonitorService.exe.config file (in Notepad) and edit your configuration as needed – save when done.
-   Now start the service (Windows..Run..services.msc, find the one named PowerMonitorService, right click and choose “Start”)

Enjoy…

GEO 51.4043388366699:\-1.2875679731369