.. WhooshTraffic documentation master file, created by
   sphinx-quickstart on Thu Apr 28 15:23:40 2011.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

==========================
API Services Documentation
==========================

Contents:

.. toctree::
   :maxdepth: 2
   
   API Docs <api>

=================
Language Wrappers
=================

We presently have only one language wrapper (for PHP) and it can be
found on our `GitHub profile <https://github.com/whooshtraffic>`_.

* `PHP API Wrapper <https://github.com/whooshtraffic/whoosh-traffic_api_php>`_

=============
Authorization
=============

Authorization with the WhooshTraffic API is simple (this applies to
all available API's); it requires credentials to be sent ``base64``
encoded using the `HTTP Basic Authorization
<http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#14.8>`_
header.

An example using Python::

       ...
       # Assuming these to be a valid login/key
       login = "4f//y1YFy5PpFLW"
       key   = "GrSCMg45heb7Y.IK.iogIOeQAi2C4yb./iPT6hnZ5UZwpTS1z4qWu"
       
       # Format and b64encode according to 14.8
       auth = base64.b64encode(login+':'+key)
       
       # Build the request
       req = urllib2.Request('https://secure.whooshtraffic.com/ranktracker/pairs')
       req.add_header('Authorization', 'Basic %s' % auth)
       req.add_header('Accept', 'application/vnd.whoosh.api+json')
       req.add_header('Range', 'offset=0')
       
       result = opener.open(req)

Please note, the term "Basic " (with a trailing space) must come
before the base64 encoded credentials! If not, the server will reject
the attempt returning a ``500 Server Error`` response.

If you send an incorrectly formatted authorization header, or none,
the server will return a ``400 Bad Request``; if you send credentials
that cannot be authenticated the server will return a ``401
Unauthorized``; and if you send valid credentials but are requesting a
resource that does not belong to you the server will return a ``404
Not Found`` response.

==================
Indices and tables
==================

* :ref:`genindex`
* :ref:`search`

