Title: Meet Moxie
Date: 30/10/2012
Summary: We're working on a new version of Mobile Oxford...

Before you can meet Moxie, a bit of a history lesson is required.

While I've only been working on Mobile Oxford since March 2012, the project originated as a JISC-funded project back in 2009 (or so). This produced an open source project 'Molly', as described on [mollyproject.org](http://mollyproject.org):

> Molly is a framework for the rapid development of information and service portals targeted at mobile internet devices.

So, what on earth does that mean? Basically, Molly is a generic version of Mobile Oxford. Other institutions can deploy an equivalent to Mobile Oxford by just wiring up their LDAP for Contact Search, the z39.50 endpoint for library search, the Met Office code for a weather Forecast. At least, that's the idea.

Over the years Molly has put on some weight, partly due to this generic nature (and some strong internal coupling) it can be quite difficult to make additions. Much of this code is perfectly fine and just needs some minor updates, for example, there is no need to rewrite z39.50 bindings or a wrapper for the Met Office website.

Finally it's time to, meet *Moxie*,

Times have changed since Mobile Oxford originated, Moxie is a response to the shift towards API/Client applications. We're developing Moxie as a HTTP API separately from the JavaScript Client. I'll save the real nitty-gritty technical details for other posts. Instead I'll leave you with a brief lay of the land.

#####[Moxie API](https://github.com/ox-it/moxie)
* Python
* Flask
* Apache Solr - primary datastore/search
* Redis - key-value store/cache/sessions

#####[Moxie JS Client](https://github.com/ox-it/moxie-js-client)
* Backbone
* Require.js
