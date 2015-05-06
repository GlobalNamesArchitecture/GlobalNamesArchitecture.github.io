---
layout: post
title: Kickoff  Meeting for Disseminating Phylogenetic Knowledge Project
date: 2015-05-06 09:02:32
author: Dima Mozzherin
categories: gna phylogeny trees nsf
---

Yesterday [Arlin Stoltzfus][arlin] organized a kickoff meeting for the project
that got funded by NSF this year - "[Collaborative Research: ABI Development: An
open infrastructure to disseminate phylogenetic knowledge][grant]". Global Names is
participating in project and I believe it will be an interesting ride.

The idea behind is pretty cool. Imagine that someone works on a group of
organisms.  They submit names of the organisms to a service and the service
builds a phylogenetic tree out of the names. When tree is created it will start
its own life similar to a repo on Github. People will be able to reuse it,
annotate it for their own purposes, create derivative trees. It would be a
pretty nice feature for [Encyclopedia of Life][eol] to see how species
belonging to a particular clade are related to each other through phylogeny.
One problem with creation of such trees is name normalization. Scientific names
can have many alternative spellings, so to find phylo-information we will need
to be able to map names from user list to names which are recognized by the
service.

I suspect that a crossmapping tool I am working on this week might be adjusted
for this particular task, but as usual -- the devil is in details and we will
find out the requirements during the design process.


[arlin]: https://github.com/arlin
[grant]: http://nsf.gov/awardsearch/showAward?AWD_ID=1458572
[eol]: http://eol.org
