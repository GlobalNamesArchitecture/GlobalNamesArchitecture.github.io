---
layout: post
title: "Starting GSOC 2015 Sysopia Project"
date: 2015-05-03 09:25:38
author: "Dima Mozzherin"
categories: "gna sysopia gsoc"
---

Our Google Summer of Code student -- [Viduranga Wijesooriya][vidur] and I had
had our firts meeting today to start [Google Summer of Code][gsoc]
[Sysopia][sysopia] project. The purpose of the project is not names, however I
do consider it to be important for EOL and for GN as it allows to spend less
time on administration of computers and more time on writing code.

The idea behind the project is to create a dashboard that allows us to see what
is going on with all computers in a system with one glance. The system shows
several metrics graphs, each of which shows information about all machines at
the same time.  By default it shows data for 24 hours, so if everything works
well it is enough for sysadmin to check out sysopia once a day to have a very
good idea about what is happening with the system from the moment Sysopia is
installed. We did install it for EOL and I find it very useful.

[![sysopia][sysopia-img]][sysopia]

Not much functionality is there yet, but graphs show well, and it is possible
to get a point data by hovering over a line, and highlight a particular machine
when hovering over the machine name in the dialog box.

Currently the only backend for sysopia is [Sensu][sensu] but we are going to
expand it to other backends after we nail down the user interface.

[vidur]: https://github.com/vpowerrc
[gsoc]: https://www.google-melange.com/gsoc/project/details/google/gsoc2015/vpowerrc/5665117697998848
[sysopia]: https://github.com/EOL/sysopia
[sysopia-img]: /images/sysopia.png
[sensu]: http://sensuapp.org/
