---
layout: post
published: true
title: Services in Flask
description: While abstracting our Flask application from the transport layer we've introduced Services...
---

When work started on [Moxie](https://github.com/ox-it/moxie/) we were using Flask's [class-based views](http://flask.pocoo.org/docs/api/#class-based-views) for most of our application logic. This is partly due to the nature of the project, we require lots of configuration so [Mobile Oxford](http://m.ox.ac.uk) could be generic enough to be deployed at other institutions like it's predecessor [Molly](http://mollyproject.org). From this need we settled on our concept of Services, whilst not strictly following the principles of SOA, I think we've found a pragmatic solution that fits our needs.

Services to us represent a logical chunk of functionality which can be (so far) configured centrally (this actually happens on a per-[blueprint](http://flask.pocoo.org/docs/blueprints/) basis). Excuse the loose terminology but that should give you an idea of the goal.

So what did we end up with? Well a good example can be found with the [OAuth1Service](https://github.com/ox-it/moxie/blob/master/moxie/oauth/services.py#L53), it doesn't rely upon any other Services nor does it rely upon executing within the request context. Although *if* we are within a request the user credentials are cached in the session object.

Consider the following configuration:

{% highlight python %}
SERVICES = {
    'twitter_spam': {
        'OAuth1Service': (('http://api.twitter.com/oauth', 'user', 'pass'), {}),
    }
}
{% endhighlight %}

Within the `twitter_spam` blueprint we can call `OAuth1Service.from_context()` and we'll get an `OAuth1Service` object instantiated with our configuration ([DI](http://en.wikipedia.org/wiki/Dependency_injection)). Following this all subsequent calls within the same application context to `from_context()` will all return the **same** object. See:

{% highlight python %}
>>> with app.app_context():
        oa1 = OAuth1Service.from_context(blueprint_name='twitter_spam')
        oa2 = OAuth1Service.from_context(blueprint_name='twitter_spam')
>>> oa1 is oa2
True
{% endhighlight %}

We're still actively working on this approach, but so far I've been pleasantly surprised with it. We have fine-grained configuration and a clear separation between application logic and the transport layer.
