---
layout: post
title: Global Names Parser v3.2.1
date: 2015-06-01 11:02:24
author: Dima Mozzherin
categories: gna parser uuid
comments: true
---

New version of [Scientific Name Parser][parser] is [out][gem]
=============================================================

This release has some backward compatibility issues with output.

Field "verbatim" is not preprocessed in any way
----------------------------------------------------

In previous versions we did strip empty spaces and new line characters around
the name to generate "verbatim" field. Now name stays the way it was entered
into the parser.

Old behavior:

"Homo sapiens    " -> ..."verbatim": "Homo sapiens"

"Homo sapiens\r\n" -> ..."verbatim": "Homo sapiens"

New behavior:

"Homo sapiens    " -> ..."verbatim": "Homo sapiens    "

"Homo sapiens\r\n" -> ..."verbatim": "Homo sapiens\r\n"

[Global Names UUID v5][uuid_blog] is added to the output as "id" field
----------------------------------------------------------------------

```json
{
    "scientificName": {
        "id": "16f235a0-e4a3-529c-9b83-bd15fe722110",
        "parsed": true,
        "parser_version": "3.2.1",
        "verbatim": "Homo sapiens",
        "normalized": "Homo sapiens",
        "canonical": "Homo sapiens",
        "hybrid": false,
        "details": [{
            "genus": {
                "string": "Homo"
            },
            "species": {
                "string": "sapiens"
            }
        }],
        "parser_run": 1,
        "positions": {
            "0": ["genus", 4],
            "5": ["species", 12]
        }
    }
}
```
Read more about UUID v5 in another [blog post][uuid_blog]

Names with underscores instead of spaces are supported
------------------------------------------------------

Such names are often used in representations of phyo-trees. Parser now
substitutes underscores to spaces during normalization phase

Normalized canonical forms do not have apostrophes anymore
----------------------------------------------------------

I am removing behavior introduced in v3.1.10 which would preserve apostrophes
in normalized version of names like "Arca m'coyi Tenison-Woods". Apostrophes
are not code compliant.

New behavior:

```json
{
    "scientificName": {
        "id": "b3a9b1a3-f73c-5333-8194-a84c6583d130",
        "parsed": true,
        "parser_version": "3.2.1",
        "verbatim": "Arca m'coyi Tenison-Woods",
        "normalized": "Arca mcoyi Tenison-Woods",
        "canonical": "Arca mcoyi",
        "hybrid": false,
        "details": [{
            "genus": {
                "string": "Arca"
            },
            "species": {
                "string": "mcoyi",
                "authorship": "Tenison-Woods",
                "basionymAuthorTeam": {
                    "authorTeam": "Tenison-Woods",
                    "author": ["Tenison-Woods"]
                }
            }
        }],
        "parser_run": 1,
        "positions": {
            "0": ["genus", 4],
            "5": ["species", 11],
            "12": ["author_word", 25]
        }
    }
}
```

[parser]: https://github.com/GlobalNamesArchitecture/biodiversity
[gem]: https://rubygems.org/gems/biodiversity
[uuid_blog]: http://globalnamesarchitecture.github.io/gna/uuid/2015/05/31/gn-uuid-0-5-0.html
