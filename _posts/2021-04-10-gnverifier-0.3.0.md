---
layout: news_item
title: GNverifier release v0.3.0
date: 2021-04-10
author: dimus
product: GNverifier
version: 0.3.0
categories: GNverifier release
comments: true
---

There are millions of checklists in use by scientists and nature enthusiasts.
Very often such lists contain misspellings or outdated names. To help our users
to clean up their checklists and monitor the quality of them we are releasing
[**GNverifier**][gnverifier] v0.3.0 written in Go language. We support
[Semantic Versioning]. Until v1.0.0 changes in the command line options, input
and output format might not be backward compatible with previous v0.x. However
the program can be used in production. Users just need to be aware that
sometimes incompatible changes might happen.

We released several implementation of name-verification
(reconciliation/resolution) before. All of them did not have enough speed for
the purposes of verifying massive lists of scientific names. This release
provides from 10x to 100x throughput improvements compare to older versions.
For example we get 2,500 names/second on average when we work with name-strings
detected in Biodiversity Heritage Library.

## Summary

[**GNverifier**][gnverifier] can help to answer the following questions:

- Is a name-string a real name?
- Is it spelled correctly, and if not, what might be the right spelling?
- Is the name currently in use?
- If it is a synonym, what data-sources consider to be currently accepted name?
- Is a name a homonym?
- What is a taxon that the name points to and where it is placed in a variety
  of classifications?

Name verification and reconciliation involves several steps

- Exact match: input name matches canonical form located in one or more
  data-sources.
- Fuzzy match: if no exact match is found, there is a fuzzy matching of
  canonical forms
- Partial Exact match: if previous 2 steps failed, we remove words from the end
  or from the middle of a name and try to match what is left, until we end up
  with a bare genus.
- Partial Fuzzy match: in case if partial exact match did not work, and the
  remained name is not uninomial, we apply fuzzy matching algorithms.

Matched results are then sorted by a scoring algorithm. The ["About"
page][about] contains more detailed information about matching and scoring.

## Performance

We observe speeds ~2,500 names per second for checklists that are coming
from optical character recognition process and contain many misspellings.

## Usages

The simplest way to use [**GNverifier**][gnverifier] is via
[web-interface][gnverifier web].  The online application emits results in HTML,
CSV and JSON formats and can process up to 5000 names per request.

For larger datasets, and as an alternative there is a [command line
application][gnverifier cli] that can be downloaded for Windows, Mac and Linux.

```bash
gnverifier file-with-names.txt
```

This version adds an option `-c` or `--capitalize` to fix capitalization of
name-srings before verification. It is especially useful for web-interface, as
it allows users "to be lazy" when they try to match names.

```bash
$ gnverifier "drsophila melanogaster" -c -f pretty
INFO[0000] Using config file: /home/dimus/.config/gnverifier.yaml.
{
  "inputId": "b20a7c40-f593-5a68-a048-0a24742b4283",
  "input": "drsophila melanogaster",
  "inputCapitalized": true,
  "matchType": "Fuzzy",
  "bestResult": {
    "dataSourceId": 1,
    "dataSourceTitleShort": "Catalogue of Life",
    "curation": "Curated",
    "recordId": "2586298",
    "localId": "69bbaee49e7c2f749ee7712f3f168920",
    "outlink": "http://www.catalogueoflife.org/annual-checklist/2019/details/species/id/69bbaee49e7c2f749ee7712f3f168920",
    "entryDate": "2020-06-15",
    "matchedName": "Drosophila melanogaster Meigen, 1830",
    "matchedCardinality": 2,
    "matchedCanonicalSimple": "Drosophila melanogaster",
    "matchedCanonicalFull": "Drosophila melanogaster",
    "currentRecordId": "2586298",
    "currentName": "Drosophila melanogaster Meigen, 1830",
    "currentCardinality": 2,
    "currentCanonicalSimple": "Drosophila melanogaster",
    "currentCanonicalFull": "Drosophila melanogaster",
    "isSynonym": false,
    "classificationPath": "Animalia|Arthropoda|Insecta|Diptera|Drosophilidae|Drosophila|Drosophila melanogaster",
    "classificationRanks": "kingdom|phylum|class|order|family|genus|species",
    "classificationIds": "3939792|3940206|3940214|3946159|3946225|4031785|2586298",
    "editDistance": 1,
    "stemEditDistance": 1,
    "matchType": "Fuzzy"
  },
  "dataSourcesNum": 28,
  "curation": "Curated"
}
```

It is possible to map a checklist to one of 100+ data-sources aggregated in
[**GNverifier**][gnverifier].

The following command will match all names from
`file-with-names.txt` against `the Catalogue of Life`

```bash
gnverifier file-with-names.txt -s 1 -o -f pretty
```

It is also possible to run web-interface locally by running:

```bash
gnverifier -p 4000
```

After running command above the interface can be accessed by a browser via
`http://localhost:4000` URL.

Complete list of `gnverifier` options can be found by running

```bash
gnverifier -h
```

## Application Programming Interface (API)

[**GNverifier**][gnverifier] does not keep all the data needed for processing
name-strings locally. It uses a [remote API][api] located at
`https://verifier.globalnames.org/api/v1`.

The RESTful API is public, it has an [OpenAPI description][api] and is
available for outside scripts.

[gnverifier]: https://github.com/gnames/gnverifier
[gnverifier cli]: https://github.com/gnames/gnverifier/releases/tag/v0.3.0
[about]: https://verifier.globalnames.org/about
[gnverifier web]: https://verifier.globalnames.org
[api]: https://app.swaggerhub.com/apis-docs/dimus/gnames/1.0.0
[Semantic Versioning]: https://semver.org
