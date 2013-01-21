Title: Services in Flask
Date: 1/11/2012
Summary: While abstracting our Flask application from the transport layer we've introduced Services...

Due to the nature of [Moxie](/meet-moxie/) and [Mobile Oxford](http://m.ox.ac.uk) we require a highly configurable application (such that it could be deployed at another institution). There were a couple of projects out there which offer this to some degree, [Flask-YAMLConfig](http://pypi.python.org/pypi/Flask-YAMLConfig) for example. Our own attempts have produced what we've rather creatively named *Services*.

Services to us represent a logical chunk of functionality which can be (so far) configured centrally (this actually happens on a per-[blueprint](http://flask.pocoo.org/docs/blueprints/) basis). Excuse the loose terminology but that should give you an idea of the goal.

So, what do Services look like? Take a look at our [`KVService`](https://github.com/ox-it/moxie/blob/master/moxie/core/kv.py#L10). Usage of this Service can be found within our Celery [tasks](https://github.com/ox-it/moxie/blob/master/moxie/places/tasks.py) and [transport views](https://github.com/ox-it/moxie/blob/master/moxie/transport/views.py). Each time the class is instantiated `from_context` this means the Service accesses its own `*args` and `**kwargs` from a per-blueprint configuration. In other words:

> The same Service accessed within calls to views registered by separate blueprints will have separate configurations.

What's more, you can safely rely on the fact that within a single request Services accessed through `from_context` will *always* return the **same** object. See the [tests](https://github.com/ox-it/moxie/blob/master/moxie/tests/test_services.py#L17) for examples of this behaviour.

The API can be further sugared by using a [LocalProxy](http://werkzeug.pocoo.org/docs/local/#werkzeug.local.LocalProxy):

    :::python
    kv_store = LocalProxy(KVService.from_context)

We're still actively working on this approach, but so far I've been pleasantly surprised with it. We have fine-grained configuration and a clear separation between application logic and the transport layer.
