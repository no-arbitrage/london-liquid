---
title: "Windows Live Writer on Vista"
date: "2006-12-03T19:42:50"
tags: [
  "dasblog"
]
---
One of the first apps I loaded onto my laptop (after paving it and installing Vista Ultimate) was Windows Live Writer (which I’m using to compose this post – on a plane at 38,000 ft on my way to Boston).

[![](WLWConfig_thumb2.png)](https://kapie.com/content/binary/WindowsLiveWriter/WindowsLiveWriteronVista_14BC2/WLWConfig4.png) So it all installed OK, I ran it and it launches straight into the config wizard (setting up your blogs) – I entered my site, username and password and hit next… Bang it fails – apparently it timed out.

OK, maybe the wrong password or something – reenter it – same thing…

Check the website is working – Yes.

Run this same config on my WinXp machine – Works OK ???

Lets look at the logging – seems to be found at :

C:Users%USERNAME%AppDataLocalWindows Live WriterWindows Live Writer.log

This gives me an error message as follows :-

WindowsLiveWriter,1.2012,None,00001,27-Nov-2006 11:32:13.063,”Unexpected exception occurred while processing resource Providers.BlogProvidersB2.xml: System.Xml.XmlException: ‘–‘ is an unexpected token. The expected token is ‘>’. Line 31, position 3.  
at System.Xml.XmlTextReaderImpl.Throw(Exception e)  
at System.Xml.XmlTextReaderImpl.DtdParserProxy.System.Xml.IDtdParserAdapter.Throw(Exception e)  
at System.Xml.DtdParser.Throw(Int32 curPos, String res, String\[\] args)  
at System.Xml.DtdParser.ThrowUnexpectedToken(Int32 pos, String expectedToken1, String expectedToken2)  
at System.Xml.DtdParser.ScanClosingTag()  
at System.Xml.DtdParser.GetToken(Boolean needWhiteSpace)  
at System.Xml.DtdParser.ParseEntityDecl()  
at System.Xml.DtdParser.ParseSubset()  
at System.Xml.DtdParser.ParseExternalSubset()  
at System.Xml.DtdParser.ParseInDocumentDtd(Boolean saveInternalSubset)  
at System.Xml.DtdParser.Parse(Boolean saveInternalSubset)  
at System.Xml.XmlTextReaderImpl.ParseDoctypeDecl()  
at System.Xml.XmlTextReaderImpl.ParseDocumentContent()  
at System.Xml.XmlTextReaderImpl.Read()  
at System.Xml.XmlLoader.LoadNode(Boolean skipOverWhitespace)  
at System.Xml.XmlLoader.LoadDocSequence(XmlDocument parentDoc)  
at System.Xml.XmlLoader.Load(XmlDocument doc, XmlReader reader, Boolean preserveWhitespace)  
at System.Xml.XmlDocument.Load(XmlReader reader)  
at System.Xml.XmlDocument.Load(Stream inStream)  
at WindowsLive.Writer.BlogClient.Providers.BlogProviderManager.ReadXmlBlogProviders(Stream blogProviders)  
at WindowsLive.Writer.CoreServices.ResourceFileDownloader.ProcessResourceSafely(String name, String resourceUrl, Int32 requiredFreshnessDays, Int32 timeoutMs, ResourceFileProcessor processor)”,””

Looks like there is a error in the XML somewhere, so I pull my feed and validate it – looks like valid RSS2.0.

Turn OFF Vista’s crazy (and plain annoying) Use Account Control (UAC) – the source for all the ‘grey screens of iritation’ I get when trying to do ANYTHING – No change.

OK, it’s trying to get config data from/for me right ? – I have that config data on my desktop (WinXP) already so lets just copy it over. Examine the WinXP desktop for the config data – find it in the registry under : HKEY\_CURRENT\_USERSoftwareWindows Live Writer : a whole raft of reg keys.

Export the whole shebang – merge it into the Vists machines registry – rerun Writer – all the config data is in there (excellent I can now post) !!

Copy the reg setings away to another drive in case I have to pave the machine again.