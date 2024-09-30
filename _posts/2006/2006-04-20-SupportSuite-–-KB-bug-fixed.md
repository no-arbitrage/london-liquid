---
title: "SupportSuite – KB bug fixed"
date: "2006-04-20T23:48:34"
tags: [
  "scripting",
  "technical"
]
---
I found an issue in SupportSuite (3.0.0.32) where if a client (user) did a KB search they would get the subject line and first 255 characters of each result match – some of these matches would be PRIVATE or DRAFT knowledgebase articles.

I guess this is a bug, but based on the responses (timeframe to even review reported issues) to bugs I reported to Kayako recently, I decided to fix it myself…

Here is what I did :

**MODIFIED /modules/core/client\_search.php**  
In the section commented as :
// ===== SEARCH ALL MODULES ========

**CHANGED**

if ($module->isRegistered(MODULE\_KNOWLEDGEBASE))
{
$\_searchresults = searchKnowledgebase($\_POST\["searchquery"\]);
}

**TO**

if ($module->isRegistered(MODULE\_KNOWLEDGEBASE))
{
$\_searchresults = searchClientKnowledgebase($\_POST\["searchquery"\]);
}

also in the **else if** block directly below :

**CHANGE**

} else if ($\_POST\["searchtype"\] == "knowledgebase" && $module->isRegistered(MODULE\_KNOWLEDGEBASE)) {
$\_searchresults = searchKnowledgebase($\_POST\["searchquery"\]);

**TO**
} else if ($\_POST\["searchtype"\] == "knowledgebase" && $module->isRegistered(MODULE\_KNOWLEDGEBASE)) {
$\_searchresults = searchClientKnowledgebase($\_POST\["searchquery"\]);

So, you've now forced the client search functions to use a new function (called **seachClientKnowledgebase**)   
that we will now define...

-   Open the file **/modules/core/functions\_clientsearch.php**
    
-   Copy the whole function named **searchKnowledgebase** and paste another copy into the same file.
    
-   Now rename the function to **searchClientKnowledgebase**.

In the newly pasted and renamed function look for the section starting with
// ========BUILD THE CATEGORY LIST FOR THIS GROUP =====

**ADD**

// KH get the status of each article and only use the 'published' ones
$\_finalpublishedarticlelist = array();
$dbCore->query("SELECT \* FROM \`". TABLE\_PREFIX ."kbarticles\` WHERE   
  *(line wrapped)* kbarticleid IN (". buildIN($\_finalkbarticleidlist) .");");
while ($dbCore->nextRecord())
{
if ($dbCore->Record\["articlestatus"\] == "published")
{
$\_finalpublishedarticlelist\[\] = $dbCore->Record\["kbarticleid"\];
}
}

**DIRECTLY AFTER**

while ($dbCore->nextRecord())
{
if ($dbCore->Record\["categorytype"\] == SWIFTPUBLIC)
{
$\_finalkbarticleidlist\[\] = $dbCore->Record\["kbarticleid"\];
}
}

Then modify the next block of code to read as follows :

if (!count($\_finalpublishedarticlelist))
{
return $\_queryresult;
}

Alternatively simply make a backup of your existing files :

-   **/modules/core/functions\_clientsearch.php**
-   **/modules/core/client\_search.php**

and replace them with the files in [php\_mods.zip](https://kapie.com/content/binary/php_mods.zip)