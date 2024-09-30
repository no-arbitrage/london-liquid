---
title: "Writing Supportable Software 2 – How easy is it to Roll Back"
date: "2006-10-28T22:00:15"
tags: [
  "development"
]
---
This is the second article on Writing Supportable Software, helping all those customers and support engineers work with our software products and troubleshoot them themselves (i.e. going as far as possible down the troubleshoot path before referring it back to you).

[You can the first article here…](https://kapie.com/Writing+Supportable+Software+1+Tracing+For+Maximum+Effect.aspx)

This article is all about what you do (what you make your software do) when it fails. So, we all know that software has bugs from time to time, one of the keys to making it *Supportable* is providing a clean option of recovery from any error / bug / crash…

This is all about thinking in manner of two stage commits. So for a two stage commit system you fire off all the individual components of a transaction, they all report back that they completed successfully and then you do the final commit of the whole thing…

Using two stage commits for all the data moving, updating of entities is essential in building supportable software and for general customer satisfaction.

I worked with an application once that wrote (a lot of) data to the file system (tens of gigabytes per process run). The original design was to stage files in the same folder that we stored the data long term and we had no effective way of doing two stage commits – the result was that when a process run failed we left the part processed data files in the long term storage folder along with all the correctly processed data – all the filenames were of a similar format so it was hugely difficult for the customer or support guys to filter out that part processed data and remove it – what’s worse is that we would reprocess the unfinished process run, if it failed again then there would be another set of part processed files with the same data in it – etc etc…

The end result of this was lots of spurious, part processed data files on the customers system, taking up hundreds of gigabytes of disk space and we couldn’t clear it out.

When I noticed this, I had the dev team change it to a two stage commit process (preventing it happening again) and had to write a trawler application to go back in and clean up the part processed files.

That is just one example, the general idea is to make sure that any failures / back outs / crashes clean up in an intelligent way or that the part processed data is stored / tagged in such a way as to be easily identifiable and ‘cleanable’ by your support guys or the customer themselves.