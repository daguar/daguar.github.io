---
layout: post
title: "Scratch Notes: Late January 2020"
---

### "Scratch Notes" (?)

First, what is going on here: Some people do
[weeknotes](https://boingboing.net/2018/07/25/deep-nerd-ruminations.html) —
regular logs of work and things of interest. I like those! But I don't find
myself personally motivated by a time-based cadence. I tend to hit some critical
mass of _stuff_ in my head that I feel some need to get down.

Hence: scratch notes. I'm considering this a low-pressure vehicle for writing in
a somewhat longer form than something like Twitter, but impressionistic, and
decidedly unpolished. We'll see! Maybe this will be the first _and_ last one,
but doing has always brought the most learning for me anyway.

### What I've been up to

After 6 years (!), last February I left Code for America. Since then I've been
rebalancing my life a bit: biking, riding trains, visiting family, taking ample
ferries.

Over the past 6 months I've also been doing some consulting and
contracting, mostly with people/orgs/companies I know.

In November, I also joined Public Digital's [Affiliates
Network](https://public.digital/2019/11/18/1-year-of-the-affiliates-network-14-new-affiliates-today/), which means I'm doing the occasional consulting engagement alongside people whose work founding the UK Government Digital Service I've long held high.

I've also been spelunking various problem areas I find interesting,
building some experimental things, and generally looking for a next _big lever_
on problems I care about. (NB: I'm not looking for a full-time gig right now, but may be
doing that in the near future.)

### Public Interest SEO

I've become _very_ interested in search quality in public interest domains
recently. In part this has been catalyzed by some analogous work I stumbled upon in the legal tech space
(see below), but really it's in part because it has the contours of a problem I
like: hidden in plain site, fairly technical/measurable, high leverage yet seemingly underpriced (seemingly not a lot
of ongoing work in the area.)

For example, if I search on Google for "food stamps" (something I happen to know
a lot/too much about) in San Francisco, we get this neat little "People Also
Ask" box that answers a question I probably have: _What is the maximum income to
qualify for food stamps?_

<img src="/assets/google-food-stamps-income-question-result.png" width="80%">

Cool! Except... **in California the gross income limit is [in fact]() 200% FPL.**
So the effect of this is that someone very well may think they shouldn't be
applying to a benefit they're eligible for. Now _that's_ a bummer!

But let's not just complain! See that little "Feedback" text in the bottom
right? We can actually tell Google what's wrong:

<img src="/assets/google-feedback-search-food-stamps.png" width="80%">

And for what it's worth, I have seen this lead to changes in prior reports. But
we need more reports!

Now I've long known about problems like this, and it's important not to look at
this as a binary: there's a spectrum of quality problems and their impacts on
people.

This one's not _great_, but it's far less bad than, say, a scam site that
takes your info (and makes you think you're applying for food stamps!) which
I've been reporting to Google about once every 6 months. (And it appears to do
something! They lose their visibility then come back with )

But what interests me _most_ about public interest search quality is as follows:

1. Fundamentally it starts with **a very tractable monitoring problem**:
   monitoring search results/rank/etc is a highly developed field given SEO's
commercial value. I'm fiddling with some approaches to doing this at scale.

2. It is a **huge wedge** for directly accessing users: the blunt truth is that,
   in general, most public interest areas don't have _awesome_ "official" content by
default. Lots of people are working on improving that (e.g., inside govt) but
there's also probably a horizontal role to exist in the space for nudging
_specific_ page quality improvements on sites that are already doing a great job
from a core content perspective, but don't have a lot of SEO capacity, and so
may lose out right now. In some areas there may even be a space for just pure
content generation: Google keywords are, after all, a pretty decent (and highly
measurable) gauge of what people are looking for — and what content needs are
unmet. What's most interesting is what you can do when you're getting a lot of
users in a niche...

More to come here hopefully. Other links of note:
- [Moz's Beginners' Guide to SEO](https://moz.com/beginners-guide-to-seo) is a
  superb resource for bootstrapping knowledge of this area
- [Margaret Hagan](https://twitter.com/margarethagan) at the Stanford Legal Design Lab has been doing 
  interesting research in this area in the legal search space

Advice and threads to pull here much appreciated! Send me [an
email](mailto:dave@fidg.org).

### Legal services technology

I've always been interested in legal aid and legal services: it's the final
backstop for a lot of people in very precarious circumstances for whom the
system has failed. Absent significantly-better systems, legal aid is often the
final recourse for things like appealing a disability determination or fighting an
eviction.

It also has many parallels to the public benefits world I've been working in the
past many years:

- A structually-underresourced system:
  [half](https://www.lsc.gov/media-center/publications/2017-justice-gap-report) the low-income Americans who
  approach an LSC-funded legal aid group will receive limited or no services due
to lack of capacity)
- Administrative burdens: complex rules, overwhelming forms, legalese language
  are all barriers for average people substantively accessing the justice
recourse to which they're entitled
- An eye towards tech to deal with these: the Legal Services Corporation funds [technology grants](https://www.lsc.gov/grants-grantee-resources/our-grant-programs/tig) at legal aid orgs, in part (I presume) acknowledging that _some_ types of automation/efficiency/usable DIY options are necessary if the access gap is to be closed

I was lucky enough to be able to make my way to this year's legal
aid technology conference in January (HT
[Toma](https://twitter.com/LawyerCommunity)) speaking on a very
practical panel doing "user experience teardowns" of legal apps (more 
[here](https://www.designforlawyers.com/)).

In some ways the area seems an extreme case of the same maladies that affect
government & public interest as they relate to technology more generally.
Whereas in other domains the problems may be at a simmer, in a context where legal
aid orgs are _so_ strapped, they're every day turning away loads of people in need (with stats like 86% of the need unmet)
, the unacceptable nature of the status quo — and the need to push, even into uncomfortable terrain — seems much clearer.

Heck, the opening remarks even included [this
line](https://www.lawsitesblog.com/2020/01/jim-sandmans-five-requirements-for-tech-to-improve-access-to-justice.html)!

_Of one thing I’m certain: An innovation initiative led by lawyers is an
oxymoron. Lawyers are not good at innovation. They are too
risk-averse, self-protective, prone to focusing on problems, and too wedded to
precedent to be able to drive change at scale._


Misc notes related to this:
- [The Stanford Legal Design Lab](http://www.legaltechdesign.com/) is doing lots
  of interesting work in this space. See for example [their work](http://justiceinnovation.law.stanford.edu/courses/design-for-justice-eviction/#2020) on design in the
evictions process
- [Upsolve](https://upsolve.org/) is a self-service DIY bankruptcy app


### Administrative burdens in the NY Times

Emily Badger and Margot Sanger-Katz have [a very neat interactive](https://www.nytimes.com/interactive/2020/01/28/upshot/administrative-burden-quiz.html) on the New York
Times site, very explicitly (look at the URL!) informed by the great work of
Pamela Herd and Don
Moynihan on _administrative burdens_.

Take a look and do the quiz. It's neat.

That said, what do I wish it did a bit differently? I believe much more in
**show, don't tell**. While taking a quiz about your postal mail habits may be
useful, it's not the same as showing the really-existing ground truth.

For example, New Hampshire Public Radio had this [great
piece](https://www.nhpr.org/post/deadline-looms-some-struggle-submit-work-hours-online-new-medicaid-requirement#stream/0) back in June that wasn't abstracting it: it had **screenshots** (from a legal aid org!) showing how a user **could not actually meet the requirements on the website**.

The power of this is that, while electeds may disagree about whether work
requirements should exist in Medicaid... probably no one thinks it should be
_impossible_ to use the web site. (For another example, in Arkansas — also
pursuing work requirements — the web site [shuts down every
night](https://www.washingtonpost.com/opinions/arkansas-says-it-wants-to-help-the-poor-its-hurting-them-instead/2018/11/19/8e61f0a2-ec3c-11e8-96d4-0d23f2aaad09_story.html).)

The "show don't tell" rule sticks with me to this day. It's why (many moons ago)
we did a thing called [CitizenOnboard](http://www.citizenonboard.com/), which
still makes the rounds! It's quite simple: showing how things are. But,
honestly, it's surprising how often that very basic dimension goes unaccounted
for in public decisionmaking processes.

(I have a longer rant about how we need to make "experience"
[legible](https://en.wikipedia.org/wiki/Seeing_Like_a_State) to the
policymaking process, but that's for another day. Moynihan and Herd's book
[Administrative
Burden](https://www.russellsage.org/publications/administrative-burden) has done
a lot in that translation area, but much more is needed — with all the caveats
that legibility is a fundamentally lossy filter that will never capture
everything.)


### Misc things

- [Monoliths are the
  future](https://changelog.com/posts/monoliths-are-the-future): a view I
wholeheartedly endorse.
- [An experiment finds EITC outreach/awareness isn't sufficient](https://www.capolicylab.org/news/report-raising-awareness-of-tax-credits-isnt-enough-to-increase-the-number-of-low-income-californians-who-claim-them/)
- [Once-weekly "tech
  shabbats"](https://en.wikipedia.org/wiki/Technology_Shabbat): been trying
this, and it is good
- ["Say
  Nothing"](https://www.amazon.com/Say-Nothing-Murder-Northern-Ireland/dp/0385521316):
I loved this book on Northern Ireland's struggles reconciling the history of
violent conflict


That's it! If this catalyzed any thoughts for you, [send me a
note](mailto:dave@fidg.org).

