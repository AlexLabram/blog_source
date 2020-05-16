---
title: Data Science is Different
author: Alex Labram
date: '2020-05-13'
slug: data-science-different
categories:
  - Data Science
tags:
  - philosophising
---

Data science is fast emerging as one of the core concepts with which companies and professionals must grapple.  However, as with most such concepts, it lives a double life: half useful technical term, half [content-free buzzword](https://marketoonist.com/2018/01/blockchain.html).

In its incarnation as a technical term, what it really represents is:

1. The convergence of data analysis techniques from a wide range of scientific fields.  Until recently, every scientific field had its own techniques and terminology.  The rise of statistical programming languages like R has both permitted and forced each field to put those into a common context, identifying and justifying any field-specific departures from emerging best practices.
2. A reaction to the traditional statistical mindset, which first starts with a model that seems intuitively appropriate and then adds epicycles until it isn't obviously wrong.  This approach is sensible when you have very little hard information to work with, but for large modern datasets it can feel like it's excusing the model rather than explaining the data.

I recently had a minor revelation that illustrated both these elements quite nicely.

## Start to end

A common hobby of armchair enthusiasts is to look over other people's work and try to impose a process diagram on it.  Data science is certainly no exception.  Historic attempts to formalise the data science process have included [CRISP-DM](https://en.wikipedia.org/wiki/Cross-industry_standard_process_for_data_mining), [KDD](http://www2.cs.uregina.ca/~dbd/cs831/notes/kdd/1_kdd.html) and Microsoft's [TDSP](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/overview).  And then there's Hadley Wickham's [little diagram](https://r4ds.had.co.nz/introduction.html), which has probably had more real-world influence than all of the acronym salad put together.

These efforts are not *entirely* without value.  Per my first point - that data science is about the convergence of different practice areas - there is merit in trying to abstract and generalise each practitioner's thought process.  Computational linguists approach problems differently to econometricians, and they're both cordially bemused by data viz specialists.  By trying to shoehorn all of these into a larger framework, we can start to understand what each specialism does well... and, crucially, what they can do better.

A little while back, as part of an internal training programme, I found myself putting together my own version.  Redrawn for this blog post in the finest image editor known to man (Powerpoint), it looks something like:

![Linear data science process diagram](/post/2020-05-13-data-science-is-different_files/Data_Science_Linear.png)

Now, this is fundamentally not a bad process diagram.  It captures most of the salient tasks that a data scientist performs, at a level of granularity that is appropriate given their relative importance.  Critically, it includes the top-and-tail elements: what happens before you load the data into R and what happens after you throw a model over the fence.  If I were trying to structure a data science project, for example for pricing purposes, this is what I'd use.

Really, its only flaw is that - as I realised recently - it is completely wrong.

## Inside to out

If you watch a data scientist in action, they don't follow a nice simple end-to-end linear process.  Rather, they proceed radially: working outwards from the data, building little pipelines behind them as they go, sometimes generating a cute graphic or pulling together a machine-learnable [tidy dataset](https://r4ds.had.co.nz/tidy-data.html).  It looks a lot more like this:

![Radial data science process diagram](/post/2020-05-13-data-science-is-different_files/Data_Science_Radial.png)

Obviously that's also a bit less complete (and less well-labelled) than my previous diagram, but the big difference between the two is in the mindset.  Rather than assuming we'll be able to shoehorn our analysis into a given framework - the data science equivalent of the much-maligned [waterfall model](https://en.wikipedia.org/wiki/Waterfall_model) in software development - we take our cue from the data and follow where it leads.  Data cleansing, reshaping, feature enineering, data visualisation, supervised learning, *un*supervised learning, model introspection: all these are just ways of drilling outwards from the data until eventually the chrysalis of noise falls away and insight flaps its wings.

The critical insight captured here - which lines up nicely with the second of the two differences I originally highlighted - is that this non-linearity isn't a bug, it's a feature.  Ultimately it's not the data's responsibility to conform to our expectations; it's our responsibility to accept it as we find it.  To quote physicist Richard Feynman's famous [report](https://en.wikipedia.org/wiki/Rogers_Commission_Report#Role_of_Richard_Feynman) on the Challenger Shuttle disaster:

> For a successful technology, reality must take precedence over public relations, for Nature cannot be fooled.

## Counting the cost

This can leave both data science practitioners and their employers in a rather uncomfortable place.  If the study of data is best treated as an open-ended scientific process rather than a waterfall-style engineering process, how can we set deadlines?  How can we measure performance?  When all our attempts to find a pattern come back with "nope", is the fault, dear Brutus, in our data or in ourselves?

These are genuine issues that have been experienced for decades by organisations, such as pharmaceuticals, that place a heavy reliance on scientific development.  It is also a common feature of start-ups, which may be [viewed](https://en.wikipedia.org/wiki/Lean_startup) as engines of business model discovery (contra established companies, which primarily execute already-discovered models).

These sectors have a decent grasp on a fundamental insight: in a scientific process, success is probabilistic.  If a drug fails clinical trials?  The company may look for ways to claw back income from the wreckage, for example by looking for other conditions it might treat, but ultimately they're prepared to suck it up.  If a start-up goes belly-up?  Some of the more successful start-up founders crashed and burned three or four (or more) times before they hit the jackpot.

Similarly, sometimes data scientists will solve the problem they were supposed to solve.  Heck, sometimes they'll even solve a completely different problem by accident - as Isaac Asimov wrote, "The most exciting phrase to hear in science, the one that heralds new discoveries, is not 'Eureka!' but 'That's funny...'".  But sometimes they'll strike out simply because the data supports no other outcome.

Of course, the random nature of this process doesn't mean we can't improve our odds.  In true [SMART](https://en.wikipedia.org/wiki/SMART_criteria) fashion, we can set more controllable intermediate objectives such as: is our analysis [understandable](https://en.wikipedia.org/wiki/Literate_programming) to a third party?  Is it [reproducible](https://en.wikipedia.org/wiki/Replication_crisis)?  Have we demonstrably tried a decent range of options?  Would another data scientist - perhaps a [peer reviewer](https://www.actuaries.org.uk/upholding-standards/actuarial-profession-standard-aps-x2) - feel we'd missed something obvious?

Unlike with engineering projects, though, where performance can often be monitored by a non-specialist project manager, all of these metrics require the business to have at least some understanding of what "good" looks like in data science.  That takes some effort, and I think a lot of real-world data science disasters stem from this one way or another.  Without a peer group, or at least a technically-literate manager, a competent data scientist is genuinely indistinguishable from a snake-oil salesman.

On the bright side, though, this means that we're probably only in the early stages of the data science revolution.  As knowledge of data science and how to manage it propagates through the body corporate, we can expect this process to unlock a slow second wave of opportunities for interesting analysis.

I should do a diagram of that...