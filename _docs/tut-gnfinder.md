---
layout: docs
title: Extract names from many PDF files
date: 2021-06-20 08:40:23
type: docs
author: dimus
permalink: /docs/tut-gnfinder/
---

## Prerequisites

- [**GNfinder**] - a scientific names finding app.
- [**GNU parallel**] - a tool for parallel execution of a command (usually it
  is either already included, or can be istalled via standard package manager
  for an OS).
- a folder with several PDF files that contain scientific names.

## Introduction

This tutorial shows how to find scientific names in many PDF files
using [**GNfinder**] and [**GNU parallel**].

If you need a powerful introduction to the command line itself, most of the
commands you will need for this [**GNfinder**] exercise, you can learn via this
[Software Carpentry Unix Shell
lesson](http://swcarpentry.github.io/shell-novice/).

## Install gnfinder and parallel

Follow instructions at [**GNfinder**] home
page for installing the application.

Check if [**GNU parallel**] is installed already, if not, follow instructions for
your OS how to install it.

## Extracting names from documents

To write this tutorial I used a folder with several PDF files in it as an
example. First, go to such folder on your own computer and create `names`
folder in it. Note that ``❯`` character designates a command line prompt and is
not part of a command.

```bash
❯ mkdir names
```

Now lets examine how many files are in the folder and how big the folder is.

```bash
❯ ls *pdf |wc -l
558
❯ du -hd0
854M .
```

So my folder contains 558 files and is 854 Megabytes in size.

Now lets use [**GNU parallel**] program to extract and verify names from these PDF
files. We are going to use the default CSV format for the output.

```bash
❯ ls *.pdf | parallel 'gnfinder  {} -v > ./names/{.}.csv'
```

The first ``ls *.pdf`` command returns filenames in the folder that end with
`.pdf`. Then we send the filenames via pipe (`|`) as an input to a [**GNU
parallel**] command. The [**GNU parallel**] checks how many CPUs the
computer has, creates the corresponding number of processes and then executes
[**gnfinder**] command in parallel for each filename using these processes. The
`{}` in the command is substituted with the name of a file, for example if pipe
received the file `Hamilton_1983.pdf`, the name of this file will be used
instead of `{}` in the command. The `{.}` is almost the same as `{}`, but it
strips the extension of the file, so `Hamilton_1983.pdf` gets to
`Hamilton_1983` and `./names/{.}.csv` becomes `.names/Hamilton_1983.csv`.

Make sure you do not overuse the parallel command, as it might overload
Apache Tika server if too many jobs are send to it simultaneously. If such
thing happens, you will start to get error messages.

On my computer the process of converting PDF to text and then running
name verification took 1 min 21 sec. Lets examine the result:

```bash
❯ cd names
❯ ls *csv |wc -l
558
❯ du -hd0
9.7M .
❯ cat * |wc -l
70909
```

Looks like for every PDF file there is now one CSV file (558 total), the total
size of the results is 9.7 Megabytes and there are 700909 rows generated in the
output.

Lets look inside one of the files:

```bash
❯ head Hamilton_1983.csv
Index,Verbatim,Name,Start,End,OddsLog10,Cardinality,AnnotNomenType,WordsBefore,WordsAfter,VerifMatchType,VerifEditDistance,VerifMatchedName,VerifMatchedCanonical,VerifTaxonId,VerifDataSourceId,VerifDataSourceTitle,VerifError
0,(RHYNCHOTA:,Rhynchota,69,80,3.67,1,NO_ANNOT,,,Exact,0,"Rhynchota Schmarda, 1859",Rhynchota,urn:lsid:irmng.org:taxname:1377138,181,IRMNG,
1,HOMOPTERA:,Homoptera,81,91,5.15,1,NO_ANNOT,,,Exact,0,"Homoptera Boisduval in Guenée, 1852",Homoptera,urn:lsid:irmng.org:taxname:1405844,181,IRMNG,
2,CICADELLIDAE),Cicadellidae,92,105,4.81,1,NO_ANNOT,,,Exact,0,Cicadellidae,Cicadellidae,3950452,1,Catalogue of Life,
3,(Macrosteles quadrilineatus,Macrosteles quadrilineatus,1025,1052,11.75,2,NO_ANNOT,,,Exact,0,"Macrosteles quadrilineatus Forbes, 1885",Macrosteles quadrilineatus,2918990,1,Catalogue of Life,
4,"Aphrodes,",Aphrodes,1208,1217,4.76,1,NO_ANNOT,,,Exact,0,Aphrodes,Aphrodes,3971201,1,Catalogue of Life,
5,"Euscelis,",Euscelis,1218,1227,4.12,1,NO_ANNOT,,,Exact,0,Euscelis,Euscelis,4044198,1,Catalogue of Life,
6,"Evacanthus,",Evacanthus,1228,1239,4.99,1,NO_ANNOT,,,Exact,0,Evacanthus,Evacanthus,4044915,1,Catalogue of Life,
7,"Limotettix,",Limotettix,1240,1251,4.75,1,NO_ANNOT,,,Exact,0,Limotettix,Limotettix,4088755,1,Catalogue of Life,
8,"Macrosteles,",Macrosteles,1253,1265,3.62,1,NO_ANNOT,,,Exact,0,Macrosteles,Macrosteles,4094664,1,Catalogue of Life,
```

It looks like in case of this file the first 9 names were found in one or more
biodiversity databases.

## Extracting text out of PDF files

It is possible to use [**GNfinder**] for extracting UTF8-encoded text out of
a large variety of files as well. Go again to the folder with PDF files and
use the following commands:

```bash
❯ mkdir texts
❯ ls *.pdf | parallel 'gnfinder {} -I > ./texts/{.}.txt'
```

Lets look at the results:

```bash
❯ cd texts
❯ ls *.txt|wc -l
558
❯ head Hamilton_1983.txt

INTRODUCED AND NATIVE LEAFHOPPERS COMMON TO THE OLD AND NEW
WORLDS (RHYNCHOTA: HOMOPTERA: CICADELLIDAE)

K. G. A. Hamilton
Biosysternatics Research Institute, Agriculture Canada, Ottawa KIA OC6

Abstract Can. Ent. 115: 473-511 (1983)

Fourteen new records of introduced leafhoppers are added to the 157 leafhoppers pre-
```

Now that you have textual respresentations of all the PDF files, you can
run [**gnfinder**] locally on these files without the use of remote services:

```bash
❯ ls *.txt | parallel 'gnfinder {} -U > {.}.csv'
```

[**GNU parallel**]: https://www.gnu.org/software/parallel/
[**gnfinder**]: https://github.com/gnames/gnfinder
[**GNfinder**]: https://github.com/gnames/gnfinder

