---
layout: news_item
title: Writing Papers as Open Source -- Solutions
date: 2015-10-31 00:15:11
author: dimus
published: false
categories: openscience
---

I decided to figure out how can I write scientific papers in truly Open Source
fashion. And here are practical decisions that allowed me to do it:

Criteria
--------

* Paper draft is under true revision control system
* Open access from the very beginning
* Open tools/standards

Solutions
---------

### Revision control

To use full power of revision control system a project should be mostly in text
format of some sort. We currently keep practically all our code on GitHub, so
Git was a natural choice.

### Open tools/formats

I decided to go with LaTeX, as it is tried and powerful markup, very well
suited for scientific writing. It allows to work with plain text, so we can
easily keep revisions in Git.

Vim is my editor of choice, but nothing prevents me or co-authors to use any
other modern text editor for LaTeX.

### Open access

With LaTeX and Git it is easy to provide early access to the work in progress,
especially with 2 commercial products that give free access for open projects
-- GitHub and OverLeaf. Overleaf supports git, although not as well as GitHub
does. So currently is it better to have GitHub as the main repository, and keep
Overleaf as a glorified viewer and use it only as a secondary repository.
Another useful tool is Mendeley for finding and organazing bibliography.

Final Result
------------

I am still learning my ropes, but excited about the progress. And the paper
about [Global Names Parser][gnparser] is now on [GitHub][gnparser-paper-github]
and [OverLeaf][gnparser-paper-overleaf]!

[gnparser-paper-github]: https://github.com/gnpapers/gnparser
[gnparser-paper-overleaf]: https://www.overleaf.com/read/zqsnwhrmpxrm
[gnparser]: https://github.com/GlobalNamesArchitecture/gnparser
