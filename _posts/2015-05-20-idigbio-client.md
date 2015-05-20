---
layout: post
title: iDigBio API Client v0.1.1
date: 2015-05-20 08:01:08
author: Dima Mozzherin
categories: idigbio api
---

In a few weeks there will be [iDigBio API hackathon][hackathon]. As I
menioned [earlier][oldpost] we decided to add another API client written in
Ruby before the hackathon starts. And [Greg Traub][greg] and I are releasing
Ruby Gem [client][gem] today.

Greg started to make ruby gem at [iDigBio][gregclient]. I took his code and
refactored it into a [Ruby gem][gem]. So now instead of 0 we have 2 Ruby
clients :smile:

This is the very first release, so if you will start using it and find
something is wrong/missing please submit an [issue][issue]. The gem uses [beta
API][api], so sometimes it might get 'stuck'. This problem will go away when
beta API will move to production.

[hackathon]: https://github.com/idigbio-api-hackathon/HackathonCentral/wiki
[oldpost]: https://globalnamesarchitecture.github.io/idigbio/hakathon/2015/04/28/idigbio-api-hackathon-meet.html
[gem]: https://github.com/GlobalNamesArchitecture/idigbio_client
[greg]: https://github.com/gete76
[gregclient]: https://github.com/iDigBio/idigbio-ruby-client
[issue]: https://github.com/GlobalNamesArchitecture/idigbio_client/issues
[api]: https://beta-search.idigbio.org/v2/
