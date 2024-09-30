---
title: "Exchange 2010 Store Compression"
date: "2009-08-14T10:56:58"
tags: [
  "technical"
]
---
[![logo-header-e2010](logo-header-e2010_thumb.gif "logo-header-e2010")](https://kapie.com/content/binary/WindowsLiveWriter/Exchange2010StoreCompression_29F5/logo-header-e2010_2.gif) One of the things I (for *I* read *we*) have been working on recently (at [my day job](http://www.c2c.com)) is looking at the ‘Store Compression’ feature in Exchange 2010.

***Store Compression*** is a new feature in [Exchange 2010](http://www.microsoft.com/exchange/2010/en/us/default.aspx) whereby some of the content of an email object is compressed as it is inserted into the Exchange Information Store (and decompressed on the way out – when it is being displayed to the user). The reason we were looking at this was we initially thought it might compete with our [MaX Compression](http://www.c2c.com/max-compression.aspx) product – a seriously fab product that transparently compresses and uncompresses attachment data in both Outlook/Exchange.

Anyway, this isn’t a sales pitch – so…

[![clip_image002](clip_image002_thumb.gif "clip_image002")](https://kapie.com/content/binary/WindowsLiveWriter/Exchange2010StoreCompression_29F5/clip_image002_2.gif)I had one of the QA guys do some testing and a side by side comparison. From the results it seemed that our [MaX Compression](http://www.c2c.com/max-compression.aspx) product still gives enormous savings – as significant as it did under Exchange 5.5 – Exchange 2007, so given the general perception that Store Compression compressed the whole email, I wondered whether the feature was actually implemented / enable in the [Exchange 2010](http://www.microsoft.com/exchange/2010/en/us/default.aspx) beta. Checking with a few MVP colleagues it seems they had been assured it was… so a bit more digging was required.

After speaking to some contacts on the Exchange team at Microsoft, it seems that the Store Compression feature *is* in the beta (MVPs are never wrong ;-)), but the feature does not compress the whole email object, as many people think – it just compresses *some elements* of the email object.

It turns out that, as you would expect, compressing and decompressing the whole email object (including attachments etc) as it goes in/out of the Information Store is *way* too processor intensive and in fact the design goal of the feature was not storage footprint reduction anyway – the original design goal was to reduce I/O throughput to the store so that the (bigger ?) goal of being able to use secondary storage for Exchange could be realized.

So, Store Compression actually only compresses the **email headers** and any **text or html body** text. This apparently gave sufficient reduction in the I/O to allow effective performance with secondary storage; it also gave a good balance of I/O optimization against CPU usage (for the overhead of compressing data).

This chart shows the kind of reductions that can achieved with [MaX Compression](http://www.c2c.com/max-compression.aspx) (or any other method of compression attachments), even with the Store Compression feature of Exchange 2010 in action. The two products/features actually work hand in hand, each compression a different aspect of the email object.

More details of [MaX Compression](http://www.c2c.com/max-compression.aspx) and [Exchange 2010](http://www.microsoft.com/exchange/2010/en/us/default.aspx), how we tested, the results and conclusions can be found [here](https://kapie.com/files/MaX_Compression_and_Exchange_2010.pdf) or on the [C2C Website](http://www.c2c.com/).

GEO 51.4043197631836:\-1.28760504722595