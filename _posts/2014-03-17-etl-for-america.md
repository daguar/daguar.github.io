---
layout: post
title: ETL for America
---

Now a few months out the intense tunnel of my [Code for America fellowship year](http://codeforamerica.org/apps/cityvoice/), I've had a bit more time and mental space to sip coffee by Lake Merritt and reflect on issues of technology and government.

### **The "experience problem"**

A mantra oft-repeated at CfA is that we're "building interfaces to government that are simple, beautiful, and easy to use."

And that _should be_ a core concern: bringing human-centered design and user experience thinking to the interfaces that government presents is important work. Governments all too often tend to privilege legal accuracy over creating an accessible, enjoyable experience for its constituents.

But this "experience problem" is really only one piece of the civic tech puzzle.

### **Data integration: a whole other can of worms**

Many of the problems government confronts with technology are fundamentally about **data integration**: taking the disparate data sets living in a variety of locations and formats (SQL Server databases, exports from ancient ERP systems, Excel speadsheets on people's desktops) and getting them into a place and shape where they're actually _usable_.

Among backend engineers, these are generically referred to as **ETL** problems, or **extract-transform-load** operations. The notion is that integrating data involves three distinct steps:

1. **Extract**: getting data _out of_ some system where it is stored, and where updates are made

2. **Transform**: reformatting and reshaping the data in ways that make it usable

3. **Load**: putting the transformed data into another system, generally something where analyzing it or combining it with other data is easy for end-users

Let's look at an example:

The Mayor's staff wants to put a simple dashboard on the City web site with building permits data. They'd like a map and some simple counts to provide a lens on local economic development to residents.

- Building permits are put into software procured in 2002 called _PermitManager_. IT staff write a script that nightly runs `permit_manager_export.exe`, which dumps the data (`permits.csv`) to a shared drive. (**extract**)

- The permit data system only contains addresses in raw text, but to map it it needs latitude and longitudes. The GIS team writes a script that every morning takes `permits.csv` and adds latitude and longitude columns based on the address text. (**transform**)

- The City has an open data portal that can generate a web map for any data set on it containing latitude and longitude. Staff write a script that uploads `permits-with-latitude-and-longitude.csv` to the open data portal every afternoon, and embed the auto-generated web map into the city's web site.

I've explained ETL in the above way plenty of times, and the thing is, almost everyone I talk to finds it easy to understand. They just hadn't thought about it too much.

And one of the foibles here is that many government staff -- particularly those at the high level -- lack the basic technical language to be able to understand the structure of the ETL problem and find and evaluate the resources out there.

The fact that I can go months hearing about "open data" without a single mention of ETL is a problem. ETL is ; it's _how_ you open data.

### **ETL: a hard ^#&@ing problem**

Did you notice in the above example that I have three mentions of city staff writing scripts? Wasn't that weird? Why didn't they use some software that automatically does this?  If you Google around about ETL, perhaps the most common question is whether one should use some existing ETL software/framework or just write one's own ETL code from scratch.

This is at the core of the ETL issue: because the very problem of data integration is about bringing together disparate, heterogeneous systems, there isn't really a clear-winner, "out-of-the-box" solution.

Couple with the fact that governments seem to have an almost vampiric thirst for clearly market-dominating, "enterprise" solutions -- c.f., the old adage that "no one ever got fired for hiring IBM" -- and you find yourself confronting a scary truth.

What's more, ETL is actually just an intrinsically hard technical problem. Palantir, a company which is _very_ good with data, essentially solves the problem by throwing engineers at it. They have a fantastic analytic infrastructure at their core, and they pay large sums of money to very smart people to do one thing: write scripts to get clients' data _into_ that infrastructure.

### **What is to be done?**

First a note on what _not_ to do: **do not try to buy your way out of this**. There is no single solution, no single piece of software you can buy for this. Anyone who tells you otherwise is is being disingenuous. And if you pay someone external to integrate 100% your data right now, you will be paying them in 11 days when you change one tiny part of your system. And I bet it will be at a mark-up.

Here are a few paths forward I see as promising:

- **Build internal capacity**: Hire smart, intellectually curious people who learn what they need to know to solve a problem. In fact, don't even start by hiring. Many of these people probably already work with you, but are hobbled by the inability to, say, install new software on their desktop, or by cultural norms that make trying out new things unacceptable.

Because data integration is a hard problem with new challenges each time you approach it, the best way to tackle them is to have motivated people who love to solve new problems, and give them the tools they need (whether that's training, software, or a supervisorial mandate.) To borrow and modify a phrase: let them run Linux.

As Andrew Clay Shafer [has said](http://www.youtube.com/watch?v=P_sWGl7MzhU): if you're not building a learning organization, you're losing to one. And I can tell you, governments are, for the most part, not building learning organizations at the moment.

- **Explore the resources out there**: I've started putting together [a list of ETL resources for government](https://github.com/daguar/ideas/issues/17). I'd love contributions. With just the knowledge of the acronym "ETL" and the basics of what it means you can start to think about how you can solve your own data problems with smaller tools (Windows Job Scheduler is analogous to In-N-Out's secret sauce.)

Because it's a generic (not just government) problem, there's also plenty of other resources out there. The data journalism folks have done a _great_ job of writing tutorials that make existing tools accessible, and we need to follow suit (and work _with_ them.)

- **Collaborate for God's sake!**: _EVERY_ organization dealing with data is dealing with these problems. And governments need to work together on this. This is where open source presents invaluable _process_ lessons for government: working collaboratively, and in the open, can float all boats _much_ higher than they currently are.

Whether it's putting your scripts on GitHub, asking and answering questions on the [Open Data StackExchange](http://opendata.stackexchange.com/), or helping out others on [the Socrata support forums](http://support.socrata.com/forums) a way to change government.

### **Wanted: government data plumbers**

In a forthcoming post, I hope to document some of the concrete experiences and lessons I've had in doing data plumbing in government, most recently some of the exciting experiments we're running in Oakland, California.

But I'm also writing this blog post -- perhaps a bit audaciously -- as a call to arms: all of us doing data work inside government need to start writing more publicly about our processes, hacks, and tools, and collaborating across boundaries.

From pure policy wonks who know just enough VBA to get stuff done to the Unix geeks piping whose `awk` knowledge strikes fear into the hearts of most sysadmins, we need to communicate more, and more publicly.

I've coded for America. It was hard, hard work, but incredibly fulfilling. So to my fellow plumbers I say: let's ETL for America.

_If these ramblings piqued your interest, I'd love to hear from you at [@allafarce](https://twitter.com/allafarce) or by [e-mail](mailto:dave@codeforamerica.org)._

