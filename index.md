# Google Summer of Code 2017

## [The Pylons Project](https://pylonsproject.org): Rework and fix WebOb's handling of HTTP Accept\* headers

The abstract of my proposal for Google Summer of Code 2017 was as follows:

> This is a proposal to rework and fix [WebOb](https://webob.org)'s handling of HTTP `Accept`, `Accept-Language`, and (if necessary) `Accept-Charset` and `Accept-Encoding` headers; check that this handling is well-tested and conforms to relevant RFCs where appropriate; and if possible, help fix a longstanding, related issue in Pyramid of unpredictable `Accept` handling during view lookup.

I began the summer reviewing the handling of the four headers, and found quite a few bugs, obsolete features, and areas where the handling did not conform to what was described in [RFC 7231](https://tools.ietf.org/html/rfc7231) &mdash; so my task became to rework and fix the handling for all four headers.

I started with the `Accept-Language` header, as it had an [open issue](https://github.com/Pylons/webob/issues/256) that needed to be resolved. We ended up spending much of the GSoC time on it, partly because it was very under-specified by [RFC 7231](https://tools.ietf.org/html/rfc7231) and [RFC 4647](https://tools.ietf.org/html/rfc4647); I blogged some notes on what I could gather from poring over the two RFCs, in the hope that they may be useful to others who are implementing the `Accept-Language` header: https://pygsoc2017.blogspot.co.uk.

This work eventually resulted in the following PR, a rewrite of the `Accept-Language` header handling, which has now been merged:

https://github.com/Pylons/webob/pull/335

This also resolves the issue in https://github.com/Pylons/webob/issues/256.

Having made decisions on many of the structural and design issues in working through the `Accept-Language` header, the rewrite for the handling of the other three headers did not take nearly as long &mdash; but unfortunately the summer is short, and there was not enough time within the GSoC period to merge. However, the handling for all four headers are complete, functional, documented and tested &mdash; they all now conform to RFC 7231. This is the PR containing the work on the four headers after the merged PR:

https://github.com/Pylons/webob/pull/338

There remain quite a few issues on which I would like to work with my mentors and the Pylons Project community to resolve:

* My mentors would like to see the documentation for the header subclasses be moved to the base class, to present a single shared API.
* I will work with my mentors to resolve any issues that come up with the not-yet-merged pull request so that it can be merged, and help deprecate the methods that need to be deprecated.
* I have updated the API documentation for the header classes, but it is on the [`webob` api page](https://webob.readthedocs.io/en/stable/api/webob.html), and not a `webob.acceptparse` page, which seems strange. The Accept\* headers' request properties are auto-documented on the [`request` api page](https://webob.readthedocs.io/en/stable/api/request.html), but I should perhaps rewrite the request properties' docstrings to make them fit in with the other docstrings on that page better. And the "Accept-\* headers" section on the [reference page](https://webob.readthedocs.io/en/stable/reference.html) is out-of-date, and describes methods we have decided to deprecate, so it needs to be updated.
* There is a significant amount of code that is repeated across the four headers, but heavily re-using code across the four headers was one of the main reasons why so many bugs were hidden. It may take some consideration to see which code we can re-use, and which would be better left alone so they are easier to read and understand.
* Pyramid will need to switch away from the methods being deprecated in WebOb in favour of the new RFC-compliant ones, and deal with any backward incompatibility issues.
* The longstanding issue of unpredictable `Accept` handling, mentioned in the abstract of the proposal, was proposed as an optional task that we could tackle if there was enough time remaining in GSoC; unfortunately there was not, but I would like to see if I can help tackle it sometime after GSoC.
* There is [a minor documentation issue with Sphinx](https://github.com/Pylons/pylons-sphinx-themes/issues/7) that came up while we were working on the `Accept-Language` header.
* After a little break, I would like to review acceptparse.py with fresh eyes, and see if there is any code I can refactor or documentation I can improve. As I review, I can also help draw up a list of changes for the changelog and any release announcements.
* We discussed the headers and RFCs over a few long GitHub issue threads, and as my understanding of the headers and RFCs has developed since then, I would like to go back to update them, as they may be useful to others. (While researching on how to tackle implementing these headers, I would often find our (very new) GitHub threads near the top of search results!)
* I have also been asked by my mentor Bert to create some documentation explaining the thinking and reasoning behind the implementation, similar to the blog posts I have written, both for people looking to see what they can do with WebOb, and as a resource for other implementers.


So this is what has been achieved, and what remains to do. Google Summer of Code has been an invaluable learning experience, and I would very much like to thank my mentors Bert, Michael and Steve for their guidance, and Google and [the Python Software Foundation](https://www.python.org/psf/) for making this possible!
