---
title: "Sandcastle Documentation Generation"
date: "2006-11-22T21:27:47"
tags: [
  "development",
  "tools"
]
---
At [work](http://www.c2c.com) I have been the champion of an Integration Framework for our Archive One product. This is a set of API’s, events and open file formats embedded into the Archive One products that allow customization ‘around the edge’.

The kind of thing that can be done with the Integration Framework is running searches, interworking with the AD schema, running archive policies, integration into Network Management Systems or Document Management Systems and the like – it really opens the product to other business systems as either the slave (other systems controlling Archive One) or the master (other systems being controlled by Archive One).

We have been writing up the API documentation recently and had initially started this as a (Word) document we would publish to customers and developers. I looked into [NDoc](http://ndoc.sourceforge.net/) which now seems dead (at least for VS2005 and .NET2.0) and then came across [Sandcastle](http://blogs.msdn.com/sandcastle/). Sandcastle is Microsoft’s answer to NDoc, it allows the collation and generation of documentation from the XML comments embedded in your code

[![](sandcastlegui_thumb%5B4%5D.png)](https://kapie.com/content/binary/WindowsLiveWriter/Sandcastle_E159/sandcastlegui%5B4%5D.png) Right now it is at CTP stage and is command line driven – a few projects have sprung up which provide a UI for it (backing on to the CLI). One is a [VS2005 add in](http://codeplex.com/SandcastleAddIn) (which I installed but it wasn’t immediately obvious how to use it, so I didn’t really pursue) and other is [SandcastleGUI](http://www.inchl.nl/SandcastleGUI/) – this provides a single form where you choose the options, point it to the folders containing the Sandcastle app, the code DLLs and where you want the docs to end up and then simply kick it off.

When you hit ‘Start Documenting’ it closes the form and fires off a command prompt window (batch files) which in turn fires off other command prompt windows until it’s collated everything and built the documentation.

It allows you to generate html documents – a whole website, multiple pages, images etc or a compiled help file (.chm) or even both if you wish.

Another of the options is ‘Online MSDN Links’ this provides a [MSDN](http://msdn.microsoft.com) link for every object type it can find in the base class library. Be warned enabling this option means it has to reflect through about 14500 classes / objects in the framework so that they can be indexed and used if found in your app / library. Enabling this option increases the time to generate from about 1 minute (on my Dual Core laptop) to about 15 minutes – I’d recommend disabling this option whilst developing the app / library and only enable it when you are doing the final or release build.

The resulting documentation is really good quality – pretty much the same format as MSDN documentation.

For the [![](doc_results_thumb%5B2%5D.png)](https://kapie.com/content/binary/WindowsLiveWriter/Sandcastle_E159/doc_results%5B2%5D.png)effort of a few extra XML comment lines in your code this provides great value and a very comprehensive resulting document.

The easiest way I have found to integrate this into VS2005 is to set it up as an External Tool.

I create a folder under the solution folder named ‘Documentation’, I run SandcastleGUI manually the first time around, do all the required configuration and then save the settings to a file in the solution directory (don’t save it in the ‘Documentation’ folder as that is cleared out with each Sandcastle run.

I have an ‘External Tool’ configured which points to SandcastleGUI with parameters of :

**/document $(ProjectDir)settings.SandcastleGUI**

(I called the settings file ‘settings.SandcastleGUI’) and called the tool :

**Sandcastle This Project**

This allows me to simply click this tool and it will go away and create all the documentation (with my chosen parameters) for the project.

[![](sandcastle_tool_thumb%5B3%5D.png)](https://kapie.com/content/binary/WindowsLiveWriter/Sandcastle_E159/sandcastle_tool%5B3%5D.png)

As I mentioned this is CTP at the moment – I look forward to the release version, hopefully fully integrated into Visual Studio and stylesheets that exactly mimic MSDN.