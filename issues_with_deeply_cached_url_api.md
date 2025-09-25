Sometimes you have a service that uses some exposed api exposed over some url.

If you have a lot of caching servers and mechanism on your side then it can happen that at some point you will be reciveing an old version (cached one) of the exposed api.

Possible issue is:

https://stackoverflow.com/questions/31653271/how-to-call-curl-without-using-server-side-cache

The producing side assures and you can see that the exposed api has the latest state, but on the consumer side, you still have the old version.

Then it becous exhausting and tedious to go through all these cascading cache servers and other caching mechanisms to find on what level this actually happens.
