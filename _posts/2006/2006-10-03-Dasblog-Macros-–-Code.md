---
title: "Dasblog Macros – Code"
date: "2006-10-03T22:04:24"
tags: [
  "dasblog"
]
---
Here’s the code for the macros I created – feel free to copy / compile it yourself. I also included [a compiled DLL here](https://kapie.com/content/binary/KensMacros.dll) – BUT it is compiled against .NET2.0 so will only work if your hosting Dasblog in a ASP.NET2.0 virtual directory.

-   Copy the compiled DLL into your /bin folder
-   Edit your Web.config file, locate the **<newtelligence.DasBlog.Macros>** node
-   Add the following element **<add macro=”khmacros” type=”KensDasblogMacros.KensMacros, KensMacros”/>**
-   Edit your template pages to include **<%TrackbackCount()|khmacros%>** or **<%TrackbackountAll()|khmacros%>** or **<%KensBlogStats()|khmacros%>**

using System;

using System.Collections;

using System.Text;

using System.Web.UI;

using newtelligence.DasBlog.Runtime;

using newtelligence.DasBlog.Web.Core;

using newtelligence.DasBlog.Util;

namespace KensDasblogMacros

{

    public class KensMacros

    {

        protected SharedBasePage sharedBasePage;

        protected Entry currentEntry;

        public KensMacros(SharedBasePage page, Entry entry)

        {

            sharedBasePage = page;

            currentEntry = entry;

        }

        public virtual Control TrackbackCount()

        {

            if (this.currentEntry != null)

            {

                int trackbackCount = 0;

                IBlogDataService dataService = sharedBasePage.DataService;

                trackbackCount = dataService.GetTrackingsFor(currentEntry.EntryId).Count;

                return new LiteralControl(trackbackCount.ToString());

            }

            return new LiteralControl(“”);

        }

        public virtual Control TrackbackCountAll()

        {

            int trackbackCountAll = 0;

            IBlogDataService dataService = sharedBasePage.DataService;

            DateTime\[\] daysWithEntries = dataService.GetDaysWithEntries(UTCTimeZone.CurrentTimeZone);

            foreach (DateTime day in daysWithEntries)

            {

                EntryCollection entries = dataService.GetEntriesForDay(day, UTCTimeZone.CurrentTimeZone, String.Empty, 1, int.MaxValue, String.Empty);

                foreach (Entry potentialEntry in entries)

                {

                    trackbackCountAll += dataService.GetTrackingsFor(potentialEntry.EntryId).Count;

                }

                return new LiteralControl(trackbackCountAll.ToString());

            }

            return new LiteralControl(“”);

        }

        public virtual Control KensBlogStats()

        {

            StringBuilder sb = new StringBuilder(  
);

            try

            {

                IBlogDataService dataService = sharedBasePage.DataService;

                DateTime monthFirst = new DateTime((DateTime.UtcNow.Year), (DateTime.UtcNow.Month), 1, 0, 0, 0);

                DateTime monthLast = new DateTime((DateTime.UtcNow.Year), (DateTime.UtcNow.Month), DateTime.DaysInMonth(DateTime.UtcNow.Year,DateTime.UtcNow.Month),23,59,59);

                DateTime weekFirst = Macros.GetStartOfCurrentWeek();

                DateTime weekLast = Macros.GetEndOfCurrentWeek();

                DateTime yearFirst = Macros.GetStartOfYear(DateTime.UtcNow.Year);

                DateTime yearLast = Macros.GetEndOfYear(DateTime.UtcNow.Year);

                int allEntriesCount = 0;

                int yearPostCount = 0;

                int weekPostCount = 0;

                int monthPostCount = 0;

                int trackbackCount = 0;

                DateTime\[\] daysWithEntries = dataService.GetDaysWithEntries(newtelligence.DasBlog.Util.UTCTimeZone.CurrentTimeZone);

                foreach (DateTime day in daysWithEntries)

                {

                    EntryCollection entries = dataService.GetEntriesForDay(day, newtelligence.DasBlog.Util.UTCTimeZone.CurrentTimeZone, String.Empty, 1, int.MaxValue, String.Empty);

                    allEntriesCount += entries.Count;

                    foreach (Entry potentialEntry in entries)

                    {

                        if (potentialEntry.CreatedUtc > monthFirst && potentialEntry.CreatedUtc <= monthLast)

                        {

                            monthPostCount++;

                        }

                        if (potentialEntry.CreatedUtc > weekFirst && potentialEntry.CreatedUtc <= weekLast)

                        {

                            weekPostCount++;

                        }

                        if (potentialEntry.CreatedUtc > yearFirst && potentialEntry.CreatedUtc <= yearLast)

                        {

                            yearPostCount++;

                        }

                        trackbackCount += dataService.GetTrackingsFor(potentialEntry.EntryId).Count;

                    }

                }

                int commentCount = dataService.GetAllComments().Count;

                sb.Append(“<div class=”blogStats”>”);

                sb.Append(“<table border=”0″ width=”100%”>”);

                sb.Append(“<tr><td align=”left”>Total Posts</td><td align=”right”>” + allEntriesCount + “</td></tr>”);

                sb.Append(“<tr><td align=”left”>This Year</td><td align=”right”>” + yearPostCount + “</tr></td>”);

                sb.Append(“<tr><td align=”left”>This Month</td><td align=”right”>” + monthPostCount + “</tr></td>”);

                sb.Append(“<tr><td align=”left”>This Week</td><td align=”right”>” + weekPostCount + “</tr></td>”);

                sb.Append(“<tr><td align=”left”>Comments</td><td align=”right”>” + commentCount + “</tr></td>”);

                sb.Append(“<tr><td align=”left”>Trackbacks</td><td align=”right”>” + trackbackCount + “</tr></td>”);

                sb.Append(“</table>”);

                sb.Append(“</div>”);

            }

            catch (Exception ex)

            {

                sharedBasePage.LoggingService.AddEvent(new EventDataItem(EventCodes.Error, “KensBlogStats Error: “ + ex.ToString(), String.Empty));

            }

            return new LiteralControl(sb.ToString());

        }

    }

}