---
layout: post
title: "iDigBio hackathon preparation"
date: 2015-04-28 21:34:40
categories: netineti gsoc
---

On June 3rd I am going to [iDigBio hackathon][idig] meeting which will be about finding
ways to enhance their API. Today there was a pre-hackathon meeting where
iDigBio folks explained what how did they implement their API, it's backend, and
how do they use their own API for their GUIs.

I had been very impress with what they have done. Backend is based on Elastic
search, API is RESTful and json based. What was a surprise for me -- the API
calls are often take pure json as arguments. It was also great to see how did
they simplified Elastic search queries for API, keeping their API queries
simple and powerful at the same time.

They also made Python and R clients for the API. So I will try to make Ruby
version of the API client before the hackathon.

[idig]: https://github.com/idigbio-api-hackathon/HackathonCentral/wiki
