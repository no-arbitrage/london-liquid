---
title: "Migrating to MSCRM using CDF"
date: "2007-06-01T12:34:41"
tags: [
  "technical"
]
---
Just didn’t work for me.

I had it all set up as per the docs, imported the data into the CDF database, tried to ‘Migrate’ and it completed but migrated no records (even though I had 8000+ in the CDF tables).

I tried both the snippets I gleaned from googling :

-   Make user the user does not have ‘Restricted Access’
-   Turn off ‘Fast Load’ mode.

Still no joy. All the ‘required’ fields are populated, the mapping wizard works correctly, but no ‘migration’. It is a real pain as there is nowhere to go next, the documentation about CDF is really poor as is the logging/trace/error output from the Migration Wizard.

Ever the pragmatist, I cracked open VS and and have started on an app which uses the CrmService webservice to import the data. Watch this space for the source when I’m done…