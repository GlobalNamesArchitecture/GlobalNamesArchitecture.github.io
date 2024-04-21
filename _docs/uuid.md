---
layout: docs
title: Scientific Names and UUIDs
date: 2024-04-21 10:54:30
permalink: /docs/sci-names-uuids/
---


# Why Scientific Names are Bad Identifiers for Computers

Scientific names are very important in biodiversity informatics, and are way
better than numbers or strange combination of characters for human to
human communication. However, they are not the best choice as global
identifiers for computers.

### Scientific name strings have different length

More often than not identifiers end up in databases and used as a primary index
to sort, connect and search data. Scientific name strings vary from 2 bytes to
more than  500 bytes in length. So if they are used as keys in database they
are inefficient, they waste a lot of space, become less efficient for finding
or sorting information -- indexes key size is usually determined by the
the largest key.

UUIDs have always the same, rather small size -- 16 bytes.  Even when UUIDs are
used in their "standard" string representation -- they are still reasonably
small -- 36 characters. Storing them in a database as a number is obviously more
efficient.

### It is hard to spot differences in name strings

It is very hard for human eye to spot the difference between strings like this

* Corchoropsis tomentosa var. psilocarpa (Harms & Loes.) C.Y.Wu & Y.Tang

* Corchoropsis tomentosa var. psilocanpa (Harms & Loes.) C.Y.Wu & Y.Tang

Much easier for their corresponding UUIDs

* 5edecb2b-903f-54f1-a087-b47b3b021fcd

* 833c664b-7d00-5c3b-97a4-98b0ab7d0a9a

### Name strings come in different encodings.

Currently Latin1, UTF-8 and UTF-16 are most popular encodings used in
biodiversity. If authorship or name itself has characters outside of the
128bits of ASCII code -- identically looking names will be quite different for
computers.

### Name strings are less stable because of their encoding

When names are moved from one database to another, from one paper to another
sometimes they do not survive the trip. If you spent any time looking at
scientific names in electronic form you did see something like this:

* Acacia ampliceps ? Acacia bivenosa

* Absidia macrospora V�nov� 1968

* Absidia sphaerosporangioides Man&lt;acute&gt;ka & Truszk., 1958

* Cnemisus kaszabi Endr?di 1964

Usually names like these had been submitted in a "wrong" encoding and some
characters in them were misinterpreted. UUID on the other hand is just a
hexadecimal number, which can be transitioned between various encodings more
safely.

### Name strings might look the same in print or on screen, but be different

* `Homo sapiens`

* `Homo sаpiens`

These two strings might look exactly the same on a screen or printed on paper,
but in reality they are different. Here are their UUIDs:

* `16f235a0-e4a3-529c-9b83-bd15fe722110`

* `093dc7f7-5915-56a5-87de-033e20310b14`

The difference is that the second name has a Cyrillic `а` character, which in
most cases will look exactly the same as Latin `a` character. And when the
names are printed on paper there is absolutely no way to tell the difference.
UUID will tell us that these two name strings are not the same.

### Nothing prevents to continue to use name strings for human interaction

One argument that people often give -- it is much easier for users to type

http://biosite.org/Parus_major

than

http://biosite.org/47d61c81-5a0f-5448-964a-34bbfb54ce8b

For most of us it is definitely true and nothing prevents developers
to create links of the first type, while still using UUIDs behind the scene.

Why UUIDs v5 are better than any other UUIDs as a scientific name identifier
---------------------------------------------------------------------------

* They can be generated independently by anybody and still be the same to the
  same name string

* They use SHA1 algorithm which does not have (extremely rare) collisions
  found for MD5 algorithm

* Same ID can be generated in [any popular language][examples] following
  [well-defined algorithm][algorithm]

[gnuuid]: https://github.com/GlobalNamesArchitecture/gn_uuid
[uuid]: http://en.wikipedia.org/wiki/Universally_unique_identifier
[motivation]: http://tools.ietf.org/html/rfc4122#section-2_
[examples]: https://github.com/GlobalNamesArchitecture/gn_uuid_examples
[algorithm]: http://tools.ietf.org/html/rfc4122#section-4.3
