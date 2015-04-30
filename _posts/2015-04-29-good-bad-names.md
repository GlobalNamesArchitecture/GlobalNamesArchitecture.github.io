---
layout: post
title: "Good names, bad names?"
date: 2015-04-29 15:52:58
author: "Dima Mozzherin"
categories: "gni resolver globi"
---

Just had a great conversation with [Jorrit Poelen][jorrit] from [GloBI][globi].
Jorrit uses [GN Resolver][resolver] to clean up names for Globi, and on top of
knowing that name exists in GN Resolver he also needs to know if the name is
actually valid.

Resolver by design contains 'good' and 'bad' names. We do need to know what
kind of misspellings exist in the wild and map information associated with them
to good names. These misspellings and outright wrong names make Jorrit&rsquo;s
life much harder, as we do not have a tool that clearly marks 'good' names as
good.  There are ways to be more or less sure if a name is good:

* If names belong to a specific clade -- use only highly curated sources
* Count how many sources know about a name
* See if a name appears at least in one curated source
* Check if name got parsed

But all these approaches are not universal, and do not give a clear answer.  So
what would be a solution?

It seems that a good solution would be to write a classifier which takes in
account all relevant features and meta-features of a name, considers them and
then puts the name into 'good' or 'bad' bucket.  Every name has several
features associated with it and we can train a Bayes classifier to make a
decision if name is 'good' or 'bad' using these features. When it is done
running through our ~20 million names -- each of them will be marked as trusted
or not.

I am pretty much sure that such classifier, especially at its first iteration
will make mistakes. How can we deal with them? Here is an idea -- when API
returns back data back to a user -- data will have two new fields -- 'trusted'
as yes/no and a URL to complain about this decision something like:

    http:/resolver.globalnames.org/trusted?name_id=123&wrong_value=1

People can just copy and paste this URL into browser, or set it as a "Report
mistake" button for every name in their results html.  If this button is pushed
GN Resolver will register human curation event and data from this event will be
used to improve performance of the classifier algorithm. Human curations will
trump computer algorithm and they can be collected in a new data source for
feedbacks...

Details of the interface can be decided later when we build the classifier. I
know that the problem of separating trusted names from untrusted is a task that
about everybody who uses resolver actively asked me about one time or another.
So who and when can build it? And now I am thinking that our Google Summer of
Code student might be interested in making it happen instead of improving
NetiNeti.  I personally think automatic curation of names is more important.

Jorrit submitted an [issue][issue] about this idea

[jorrit]: https://github.com/jhpoelen
[globi]: http://www.globalbioticinteractions.org/about.html
[resolver]: http://resolver.globalnames.org
[issue]: https://github.com/GlobalNamesArchitecture/gni/issues/38
