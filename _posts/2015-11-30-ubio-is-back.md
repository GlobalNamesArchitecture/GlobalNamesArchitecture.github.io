---
layout: news_item
title: uBio and Nomenclator Zoologicus are back
date: 2015-11-30 17:54:44
author: dimus
categories: uBio
comments: true
---

[uBio](http://ubio.org) and [Nomenclator Zoologicus online](http://ubio.org/NomenclatorZoologicus) experienced difficulties this year and had been down a
lot lately, mostly because there is no system administrator to look for it at
Marine Biological Laboratory anymore.

I moved uBio from old hardware at Marine Biological Laboratory to Google
Container Engine, and it is running again. Some functionality is not back yet,
mostly due to some hard-coded configuration parameters in files.  I hope
problems with the code will be fixed eventually by interested parties (I do
not plan to rewrite the code). I'll coordinate my efforts with Dave Remsen and
Patrick Leary, and hopefully together we will preserve uBio for the community.

Please note that being a system administrator for uBio is not part of my job.
I like the project, I consider it to be a 'precursor' of GN, I will try my
best to keep it running on my spare time. MBL/WHOI library pays for the cloud.

Docker containers to run uBio are located at
[dockerhub](https://hub.docker.com/u/mblab/dashboard/). We use Docker and
Kubernets at Google Container Engine to keep in alive.
