---
title: "MSCRM Migrating Accounts"
date: "2007-07-03T20:51:25"
tags: [
  "technical"
]
---
So after some [poor experiences with the MSCRM Data Migration framework](https://kapie.com/2007/06/01/Migrating+To+MSCRM+Using+CDF.aspx) I decided to get pragmatic and write a C# app to do the migration.

The CDF is poorly documented at best, it seems they (Microsoft) give you a bunch of database tables, an Excel spreadsheet outlining the schema and a ‘Good Luck’. There is little ‘googleable’ (is that a word) knowledge about it either.

The good news was that the MSCRM SDK is much better documented (on MSDN). There is not a lot of googleable info around but there is enough ([Stunnware](http://www.stunnware.com/crm2/) proved pretty helpful for me).

There were other challenges also – the software we purchased for exporting the ACT! 2000 data to Access ([Exporter Pro](http://www.crmaddons.com/p-15-export-pro.aspx)) did a good job of getting the data out of ACT but the Unique ID left a bit to be desired, they are basically a munge of punctuation characters and alphanumerics – what’s wrong with a GUID or a int ??

So, anyway, I got there in the end…

The connecting to the CrmService was pretty easy, as was the population and addition of an account.

            crmSvc = new CrmService();

            crmSvc.Url = MsCrmUrl;

            crmSvc.Credentials = new System.Net.NetworkCredential(CrmUsername, CrmPassword, CrmDomain);

            account acct = new account();

            acct.name = “company name”;

            acct.address1\_line1 = “address line one”;

            acct.address1\_line2 = “address line two”;

            acct.address1\_line3 = “address line three”;

            // etc

            Guid acctGuid = crmSvc.Create(acctGuid);

Simple as that – do it for each account…

Next installment will outline adding Contacts an then linking them to an account.