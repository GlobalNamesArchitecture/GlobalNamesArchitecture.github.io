---
layout: news_item
title: gnparser release (v0.12.0) can be used in most modern languages
date: 2019-11-19 15:01:13
author: dimus
version: 0.12.0
categories: gnparser release
comments: true
---

A few days ago we released v0.12.0 of [gnparser (Go
version)](https://gitlab.com/gogna/gnparser/-/releases). This version made it
possible to compile gnparser algorithms into [C-compatible
library](https://gitlab.com/gogna/gnparser/blob/master/binding/main.go). Such
library makes it possible to use gnparser with its native speeds in any
language that supports binding to C. Such languages inlude Python, Ruby, Java
(via JNI), Rust, C, C++ and many others.

We already updated [Ruby's biodiversity parser
gem](https://github.com/GlobalNamesArchitecture/biodiversity) to take benefit
of a dramatic speed increase and parsing quality of `gnparser`.

Here are a quick benchmarks that compare how `biodiversity` performs before
and now:

| Program      | Version | Full/Simple | Names/min |
| ------------ | ------- | ----------- | --------: |
| gnparser     | 0.12.0  | Simple      | 3,000,000 |
| biodiversity | 4.0.1   | Simple      | 2,000,000 |
| biodiversity | 4.0.1   | Full JSON   |   800,000 |
| biodiversity | 3.5.1   | n/a         |    40,000 |

With this improved speed Encyclopedia of Life, which is written in Ruby,
can process all their names using Ruby in less than 15 minutes.

README file of `gnparser` contains [instructions](https://gitlab.com/gogna/gnparser#use-as-a-shared-c-library) how to make such c-shared
library and `biodiversity` code is a good example of connecting the library to
other languages.
