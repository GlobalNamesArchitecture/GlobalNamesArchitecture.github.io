---
layout: post
title: New tool to crossmap checklists
date: 2015-05-11 11:39:02
author: Dima Mozzherin
categories: gna resolver checklist
---

Yesterday I released a new command line tool for name resolution called
[gn_crossmap][gncrossmap]. It is designed for people who work with checklists
of scientific names using a spreadsheet software (MS Excel, Apple Numbers, Open
Office, Libre Office, Google Sheets etc.) and want to compare names that they
have with another reference source. The program takes a spreadsheet saved as
csv file as input and generates another csv-based spreadsheet with resolution
data.  [Examples of input and output][examples] are included into the code.
[README][readme] file describes how to use the project from a command line or
as a Ruby library.

This program requires internet connection, Ruby >= 2.1 installed on the machine.

Basic usage is:

    $ gem install gn_crossmap
    $ crossmap -i input.csv -o ouput.csv -d 1

where

---

short| long attr        | Description
-----|------------------|----------------------------------------------------------------------------------------
-i   | --input          | checklist's spreadsheet saved as csv file
-o   | --output         | path to the output file. Default is output.csv in the current directory
-d   | --data-source-id | ID of one of the GN Resolver [data sources][ds], Catalogue of Life id (1) is default

---

Web interface to this program is also [in works][checklist]

This project started at the Catalogue of Life workshop in Leiden,
which happened in March 2015. The main focus of the hackathon was to figure out
how to help national checklists to create, maintain and compare data in their
checklists.  We determined 3 main approaches

1. **Crossmapping** checklists against other checklists and/or reference sources
2. **Annotation** of crossmapped data -- ability to share metadata, report mistakes
3. **Distribution of species** -- how to fix occurance errors for a country

A hackathon group which worked on crossmapping produced a
[code][hackathon_crossmap] which would compare checklists against Catalogue of
Life. The `gn_crossmap` program I am releasing is based heavily on what we
learned during the hackathon. Crossmaping code is mostly based on use cases
from [Rui Figueira][rui] and [Wouter Koch][wouter]. During the hackathon we
also determined ways to improve quality of name resolution further by:

* Using infraspecies' rank (var., f. subsp. etc) in the matching and penalize
  score if ranks are different
* Taking in account if matching authors are basyonym or combination authors
* Using meta-information attached to names via sensu..., not ... etc.
  to distinguish name usages

[gncrossmap]: https://github.com/GlobalNamesArchitecture/gn_crossmap
[examples]: https://github.com/GlobalNamesArchitecture/gn_crossmap/tree/master/spec/files
[readme]: https://github.com/GlobalNamesArchitecture/gn_crossmap/blob/master/README.md
[hackathon_crossmap]: https://github.com/Sp2000/hackathon_group1
[rui]: https://github.com/rpfigueira
[wouter]: https://github.com/WouterKoch
[ds]: http://resolver.globalnames.org/data_sources
[checklist]: https://github.com/GlobalNamesArchitecture/checklist
