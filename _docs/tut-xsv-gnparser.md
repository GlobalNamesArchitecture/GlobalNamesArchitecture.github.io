---
layout: docs
title: Parse names in CSV file
date: 2021-04-02 19:09:34
type: docs
author: dimus
permalink: /docs/tut-xsv-gnparser/
---

This tutorial shows how to work with big CSV files using command line
applications.

Quite often we need to extract canonical forms of scientific names, for example
to compare one checklist with another. We also want to know if names in
a checklist are well-formed. In this tutorial we will use [gnparser] and [xsv]
command line applications to quickly go through names in a CSV file and create
an output that combines original data with parsed information.

[xsv] is a very fast and powerful application to access and operate data in
CSV format.

[gnparser] is a biodiversity informatics application for parsing scientific
names and extracting canonical forms, aurhorships, etc.

## Install xsv and gnparser

Follow instructions at [xsv][xsv install] and [gnparser][gnparser install] home
pages for installing the applications.

If a checklist exists in Excel or Google Doc, save it as UTF-8 encoded CSV file
on a disk.  Here we are going to use [names.zip] file as an example.

## Examine CSV file

[names.zip] contains `names.csv` --- a well-formatted CSV file. At first we
will examine its content:

Lets see how many lines it has.

```bash
# wc counts characters, letters and lines
# -l flag means wc will only return lines number
# $ indicates a command line prompt
$ wc -l names.csv
100001 names.csv
```

So we can see the file is rather big and contains about 100 thousand records.
Lets look at the first few records using ``head`` command.

```bash
$ head names.csv
Id,ScientificName
1,Eutobrilus annetteae Joubert & Heyns 1979
2,Eburodacrys callixantha Bates 1872
3,Agonopterix arenella Denis & Schiffermüller 1775
4,"Xanthophenax pacificator Rousse & Villemant, 2012"
5,Carabus (Archiplectes) pseudopshuensis Zamotajlov 1991
6,Meropliosepsis sexsetosa Duda 1926
7,Melanopsis acanthicoides Hoernes 1876
8,Thalassironus de Man 1889
9,Bruelia nawabi Ansari 1957
```

OK. Looks like it is a CSV file. It has a header and contains two fields: `Id`
and `ScientificName`. If the `ScientificName` values contain a comma, they
are surrounded by quotes.

```csv
4,"Xanthophenax pacificator Rousse & Villemant, 2012"
```

To parse such file we have to extract the second CSV field with names, get
rid of the header and remove quotes. We can use `xsv` application for such task.

`xsv` has many very useful commands. Here we are going to use `xsv select` and
`xsv fmt`. We are also going to use command line pipes `|`: they allow to
redirect output of one command into input of the next command.

```bash
$ head names.csv|xsv select ScientificName|tail -n +2|xsv fmt -t '\t'
Eutobrilus annetteae Joubert & Heyns 1979
Eburodacrys callixantha Bates 1872
Agonopterix arenella Denis & Schiffermüller 1775
Xanthophenax pacificator Rousse & Villemant, 2012
Carabus (Archiplectes) pseudopshuensis Zamotajlov 1991
Meropliosepsis sexsetosa Duda 1926
Melanopsis acanthicoides Hoernes 1876
Thalassironus de Man 1889
Bruelia nawabi Ansari 1957
```

`head names.csv` gave us first 10 lines of a file.

`xsv select ScientificName` picked the second field out of these 10 lines.

`tail -n +2` excluded the first line (the header).

`xsv fmt -t '\t'` changed output from comma-separated to tab-separated.

Even if we have only one column, [xsv] still follows CSV format and surrounds
names containing comma with double quotes. But if we change Comma Separated
Format to Tab Separated Format, names with commas will not need to be
surrounded with quotes.

## Parsing names

Now we can use [gnparser] to parse the names from the whole file.

```bash
$ xsv select ScientificName names.csv|tail -n +2|xsv fmt -t '\t'|gnparser > parsed.csv
2021/04/02 20:04:20 Parsing 50000-th line
2021/04/02 20:04:21 Parsing 100000-th line
```

[gnparser] returns parsed results in exactly same order as the original file. Lets examine the parsing result.

```bash
$ wc -l parsed.csv
100001 parsed.csv
```

```bash
$ head parsed.csv
Id,Verbatim,Cardinality,CanonicalStem,CanonicalSimple,CanonicalFull,Authorship,Year,Quality
3a0d3ba0-6184-5878-8d0e-139a23d10d00,Eutobrilus annetteae Joubert & Heyns 1979,2,Eutobrilus annette,Eutobrilus annetteae,Eutobrilus annetteae,Joubert & Heyns 1979,1979,1
32635a0f-fc13-53c3-80b7-fde50fefd5dc,Eburodacrys callixantha Bates 1872,2,Eburodacrys callixanth,Eburodacrys callixantha,Eburodacrys callixantha,Bates 1872,1872,1
d5de8cf3-709e-5e8a-8aea-2c1074d1880c,Agonopterix arenella Denis & Schiffermüller 1775,2,Agonopterix arenell,Agonopterix arenella,Agonopterix arenella,Denis & Schiffermüller 1775,1775,1
d3e10f1e-fb0c-5480-80b3-bc4417b37cf1,"Xanthophenax pacificator Rousse & Villemant, 2012",2,Xanthophenax pacificator,Xanthophenax pacificator,Xanthophenax pacificator,Rousse & Villemant 2012,2012,1
61d1c25f-c675-5dfb-ba21-a27a420228a5,Carabus (Archiplectes) pseudopshuensis Zamotajlov 1991,2,Carabus pseudopshuens,Carabus pseudopshuensis,Carabus pseudopshuensis,Zamotajlov 1991,1991,1
2cdb73b7-09d1-5605-82ce-e7742200db92,Meropliosepsis sexsetosa Duda 1926,2,Meropliosepsis sexsetos,Meropliosepsis sexsetosa,Meropliosepsis sexsetosa,Duda 1926,1926,1
c93c5a7e-f873-5474-b201-6ff4db8db475,Melanopsis acanthicoides Hoernes 1876,2,Melanopsis acanthicoid,Melanopsis acanthicoides,Melanopsis acanthicoides,Hoernes 1876,1876,1
6db22e72-bb8a-5c0c-96c4-2621e3afcfb4,Thalassironus de Man 1889,1,Thalassironus,Thalassironus,Thalassironus,de Man 1889,1889,1
04d34702-d231-5309-89c6-b9b837245954,Bruelia nawabi Ansari 1957,2,Bruelia nawab,Bruelia nawabi,Bruelia nawabi,Ansari 1957,1957,1
```

We discarded the first line with headers from names.csv. However gnparser
inserted its own headers line, so we ended up with the same 100,001 lines. It means now we can merge the lines from `names.csv` and `parsed.csv` together.

To achieve that we will use `paste` command.

```bash
$ paste -d ',' names.csv  parsed.csv|head
Id,ScientificName,Id,Verbatim,Cardinality,CanonicalStem,CanonicalSimple,CanonicalFull,Authorship,Year,Quality
1,Eutobrilus annetteae Joubert & Heyns 1979,3a0d3ba0-6184-5878-8d0e-139a23d10d00,Eutobrilus annetteae Joubert & Heyns 1979,2,Eutobrilus annette,Eutobrilus annetteae,Eutobrilus annetteae,Joubert & Heyns 1979,1979,1
2,Eburodacrys callixantha Bates 1872,32635a0f-fc13-53c3-80b7-fde50fefd5dc,Eburodacrys callixantha Bates 1872,2,Eburodacrys callixanth,Eburodacrys callixantha,Eburodacrys callixantha,Bates 1872,1872,1
3,Agonopterix arenella Denis & Schiffermüller 1775,d5de8cf3-709e-5e8a-8aea-2c1074d1880c,Agonopterix arenella Denis & Schiffermüller 1775,2,Agonopterix arenell,Agonopterix arenella,Agonopterix arenella,Denis & Schiffermüller 1775,1775,1
4,"Xanthophenax pacificator Rousse & Villemant, 2012",d3e10f1e-fb0c-5480-80b3-bc4417b37cf1,"Xanthophenax pacificator Rousse & Villemant, 2012",2,Xanthophenax pacificator,Xanthophenax pacificator,Xanthophenax pacificator,Rousse & Villemant 2012,2012,1
5,Carabus (Archiplectes) pseudopshuensis Zamotajlov 1991,61d1c25f-c675-5dfb-ba21-a27a420228a5,Carabus (Archiplectes) pseudopshuensis Zamotajlov 1991,2,Carabus pseudopshuens,Carabus pseudopshuensis,Carabus pseudopshuensis,Zamotajlov 1991,1991,1
6,Meropliosepsis sexsetosa Duda 1926,2cdb73b7-09d1-5605-82ce-e7742200db92,Meropliosepsis sexsetosa Duda 1926,2,Meropliosepsis sexsetos,Meropliosepsis sexsetosa,Meropliosepsis sexsetosa,Duda 1926,1926,1
7,Melanopsis acanthicoides Hoernes 1876,c93c5a7e-f873-5474-b201-6ff4db8db475,Melanopsis acanthicoides Hoernes 1876,2,Melanopsis acanthicoid,Melanopsis acanthicoides,Melanopsis acanthicoides,Hoernes 1876,1876,1
8,Thalassironus de Man 1889,6db22e72-bb8a-5c0c-96c4-2621e3afcfb4,Thalassironus de Man 1889,1,Thalassironus,Thalassironus,Thalassironus,de Man 1889,1889,1
9,Bruelia nawabi Ansari 1957,04d34702-d231-5309-89c6-b9b837245954,Bruelia nawabi Ansari 1957,2,Bruelia nawab,Bruelia nawabi,Bruelia nawabi,Ansari 1957,1957,1
```

`paste -d ','` sets a comma as a delimiter when it merges lines from two files
together.

We can see here that headers and lines seem to merge correctly, and now we
can pick only fields that we care about for our final result.

## Extract required fields

If we look at the merged header, we can spot a problem: now there are two
fields with the same name: `Id`. If we want only one of them, we can achieve it
in two different ways:

- We can add an index to the field: `xsv select Id[0]` for the first one
  (indices start the count from `0`).

- We can use field position instead of the field name: `xsv select 1` (fields
  start the count from `1`).

```bash
paste -d ',' names.csv  parsed.csv|xsv select Id[0],ScientificName,Cardinality,CanonicalSimple,Quality > final.csv
```

## Examine final data

Now we have a file that contains original data together with canonical form,
name cardinality, and parsing quality. Lets try to search for a particular
data.

1. Find names that were parsed as trinomials.

    ```bash
    $ xsv search -s Cardinality '3' final.csv|head
    Id,ScientificName,Cardinality,CanonicalSimple,Quality
    32,Dorylus (Anomma) nigricans subsp. burmeisteri,3,Dorylus nigricans burmeisteri,1
    35,Malayepipona assamensis subsp. manipurensis Giordani Soika 1995,3,Malayepipona assamensis manipurensis,1
    62,Ipomoea retropilosa subsp. cundinamarcana,3,Ipomoea retropilosa cundinamarcana,1
    106,Lindernia dubia var. anagallidea (Michx.) Cooperr.,3,Lindernia dubia anagallidea,1
    172,Ballota nigra subsp. foetida Vis. 1753,3,Ballota nigra foetida,1
    180,Dendrobaena veneta subsp. veneta Rosa 1886,3,Dendrobaena veneta veneta,1
    217,Pheidole megacephala subsp. impressifrons,3,Pheidole megacephala impressifrons,1
    221,Ardonis (Chloroclystis) filicata subsp. mochleutes Prout 1958,3,Ardonis filicata mochleutes,1
    236,Camponotus perrisi subsp. jucundus,3,Camponotus perrisi jucundus,1
    ```
2. Find if there are names that were parsed as pentanomials!

    ```bash
    $ xsv search -s Cardinality '5' final.csv|head
    Id,ScientificName,Cardinality,CanonicalSimple,Quality
    13261,Poa secunda subsp. juncifolia var. juncifolia var. juncifolia (Scribn.) Soreng,5,Poa secunda juncifolia juncifolia juncifolia,1
    27014,New endemic Fusarium species hitch-hiking with pathogenic Fusarium strains causing Panama disease in small-holder banana plots in Indonesia,5,New endemic species hitch-hiking with,4
    59457,"Discovering the still unexplored arachnofauna of the National Park of Dadia-Lefkimi-Soufli, NE Greece: a taxonomic review with description of new species",5,Discovering the still unexplored arachnofauna,4
    ```

    As we can see one of the names does have 5 elements, and two others are not
    real names and they have parsing quality=4 (the worst quality).

3. Find names that parser could not parse (Quality = 0).

    ```bash
    $ xsv search -s Quality '0' final.csv|head
    Id,ScientificName,Cardinality,CanonicalSimple,Quality
    487,andersoni Blanford 1869,0,,0
    2946,(Fovephedrus) Chen 1986,0,,0
    4763,"""Orthomorpha"" oatesii Pocock 1895",0,,0
    5328,vagans Verhoeff 1905,0,,0
    5664,cavernicola Verhoeff 1937,0,,0
    5873,brahmakundensis Godwin-Austen 1915,0,,0
    7464,Cynolebiasinae.,0,,0
    7521,mandschuria Verhoeff 1936,0,,0
    7771,"Convolvulus scoparius L. f., Suppl. Pl. 135. 1782 ["" 1781 ""]. (Linnaeus 1782",0,,0
    ```

4. Find names that were parsed with significant problems (Quality = 4).

    ```bash
    $ xsv search -s Quality '4' final.csv|head
    Id,ScientificName,Cardinality,CanonicalSimple,Quality
    76,"Atheta ((Datomicra)) particula Brunke, Klimaszewski, Dorval, Bourdon, Paiero & Marshall, 2012, New Ontario Record",1,Atheta,4
    118,"Ophiogaleus , Thuy 2013",1,Ophiogaleus,4
    132,"Trichillum (Trichillum) cordobense Vaz-De-Mello & Génier, 2005",2,Trichillum cordobense,4
    262,"Hypsibius dujardini subsp. sensu Kaczmarek, Michalczyk & Mcinnes, 2015",2,Hypsibius dujardini,4
    283,Coleoptera  1758,1,Coleoptera,4
    324,"Chlaenius ((Lithochlaenius)) propeagilis Valim, Kavanaugh, Shi & Liang, 2011",1,Chlaenius,4
    359,"Petermattinglyius (Petermattinglyius) Reinert, Harbach & Kitching, 2009, SUBGEN. NOV.",1,Petermattinglyius,4
    397,Graphium megarus subsp. megarus megarus (Westwood 1844,4,Graphium megarus megarus megarus,4
    448,Ophelina abranchiata NHM _ 1769,2,Ophelina abranchiata,4
    ```

    If we want to find out why a name got Quality=4 we can parse it with more
    details:

    ```bash
    $ gnparser "Hypsibius dujardini subsp. sensu Kaczmarek, Michalczyk & Mcinnes, 2015" -f pretty
    {
      "parsed": true,
      "quality": 4,
      "qualityWarnings": [
        {
          "quality": 4,
          "warning": "Unparsed tail"
        }
      ],
      "verbatim": "Hypsibius dujardini subsp. sensu Kaczmarek, Michalczyk \u0026 Mcinnes, 2015",
      "normalized": "Hypsibius dujardini",
      "canonical": {
        "stemmed": "Hypsibius duiardin",
        "simple": "Hypsibius dujardini",
        "full": "Hypsibius dujardini"
      },
      "cardinality": 2,
      "tail": " subsp. sensu Kaczmarek, Michalczyk \u0026 Mcinnes, 2015",
      "id": "a17d9cc2-456d-56f3-9605-9ac22bf840ea",
      "parserVersion": "v1.1.0-4-g3676b8a"
    }
    ```

    Looks like [gnparser] could not finish parsing and returned a warning
    `Unparsed tail`. It would expect an infraspecific epithet after `subsp.`,
    but received `sensu` instead.

[xsv]: https://github.com/BurntSushi/xsv
[xsv install]: https://github.com/BurntSushi/xsv#installation
[gnparser]:https://github.com/gnames/gnparser
[gnparser install]: https://github.com/gnames/gnparser#installation
[names.zip]: https://github.com/dimus/tutorials/raw/master/xsv_parser/names.zip
