---
layout: news_item
title: GNverifier release v0.6.2, Advanced Search
date: 2021-12-12 Sun
author: dimus
product: GNverifier
version: 0.6.2
categories: GNverifier release
comments: true
---

[GNverifier] can help to answer the following questions:

- Is a name-string a real name?
- Is it spelled correctly, and if not, what might be the correct spelling?
- Is the name currently in use?
- If it is a synonym, what data sources consider to be currently accepted names?
- Is a name a homonym?
- What is a taxon that the name points to, and where is it placed in various
  classifications?

Biannual [GNverifier] database update is done for 14 datasets, new datasets are
pending to be added.

The [v0.6.2] version of [GNverifier] is out. It brings new features and some
changes in API, input and output format. The main change is an ability to
perform a search by name details, such as an abbreviated genus, author, year.

For example `g:M. sp:galloprovincialis au:Oliv. y:-1800` will search for a
name with genus starting with `M`, `galloprovincialis` as a specific epithet,
an author starting with `Oliv` and a year earlier or equal to `1800`.

Both name-verification and search return the same format of results. Because of
that we needed to change the name of some fields in the output, so their
meaning would correspond to both verification and search outputs.

* `inputId` changed to `id`
* `input` changed to `name`
* `preferredResults changed to `results`

GNverifier's [API v1](https://apidoc.globalnames.org/gnames) still can be used
(it did not undrgo any format changes), but the [web-page] and command line app
`gnverifier` moved to use new
[API v0](https://apidoc.globalnames.org/gnames-beta). When the new API
stabilizes, it will be renamed to API v2.

## Install GNverifier with Homebrew

```bash
brew tap gnames/gn
brew install gnverifier
```

or

```bash
brew upgrade gnverifier
```

## Changes in functionality since v0.3.0

- [web-page] shows the date when a name was imported into [GNverifier] database.
- to make it easier to cite [GNverifier], there is a [DOI information] on
  its [GitHub page][GNverifier].
- tab-delimited values (TSV) format is now supported.
- added [AlgaeBase] to data-sources.
- there are now options to return all matched results (use with caution, as
  the output might be excessively big).
- show score details in the output and on the [web-page].
- [advanced search] is added to both command line and web-based user interfaces.

## Deprecation of services.

[GNI] is the oldest version of GN name-verification algorithms. Most of its
functionality now exists in [GNverifier], so the [GNI web-site][GNI] is
going to be removed in the beginning of 2022.

The [Scala version of GNI](https://index.globalnames.org) will also be
scheduled for removal.


[GNresolver](https://resolver.globalnames.org) will continue to run the longest.
It will not be deprecated until [GNverifier] API v2 will be released.

If you use old systems, consider switching to [**GNverifier**][gnverifier]
because older systems will eventually be deprecated and stopped.

[GNverifier]: https://github.com/gnames/gnverifier
[v0.6.2]: https://github.com/gnames/gnverifier/releases/tag/v0.6.2
[about]: https://verifier.globalnames.org/about
[web-page]: https://verifier.globalnames.org
[DOI information]: https://zenodo.org/record/5774421#.YbXkDYpMFOQ
[AlgaeBase]: https://www.algaebase.org
[advanced search]: https://github.com/gnames/gnverifier#advanced-search
[GNI]: http://gni.globalnames.org
