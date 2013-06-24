=============================
GET a single URL/Keyword pair
=============================

The endpoint path for this API is
``https://secure.whooshtraffic.com/ranktracker/pairs/{id}``

GET one and GET all require an ``Accept`` header of either these two
types::

       Accept: application/vnd.whoosh.api+json

or::

       Accept: application/vnd.whoosh.api+xml

An example in Python::

       ...
       # Assuming these to be a valid login/key
       login = "Rm7YCcTm12rHcrQ"
       key   = "UKBWgXO1YPPItM5Rs8vq1uO5g2YKcvlqQO9n9XtBF1AE1rRofPUk2"
       
       # Format and b64encode according to 14.8
       auth = base64.b64encode(login+':'+key)
       
       # Build the request
       req = urllib2.Request('https://secure.whooshtraffic.com/ranktracker/pairs/935')
       req.add_header('Authorization', 'Basic %s' % auth)
       req.add_header('Accept', 'application/vnd.whoosh.api+json')
       
       # Assuming the SSL verifying opener
       result  = opener.open(req).read()
       decoded = json.loads(result)

The JSON result::

       {"id": 935, "keyword": "parnell springmeyer", "url": "http://ixmat.us", "modified": 1303899644, "locale" : null, "created": 1303892888}

====================================================
GET all (or a range of) configured URL/Keyword pairs
====================================================

The endpoint path for this API method is:
``https://secure.whooshtraffic.com/ranktracker/pairs``

GET one and GET all require an ``Accept`` header of either these two
types::

       Accept: application/vnd.whoosh.api+json

or::

       Accept: application/vnd.whoosh.api+xml

If you DO NOT specify a ``Range`` header this method will return ALL
of your pairs! If you have a large number of pairs, the response time
may be slow.

If you do specify a ``Range`` header with an offset argument it will
return a default of 10 results per set. The Range header must be
supplied in this format::

       Range: offset=0

If you wish to control the *limit*; you can specify a limit argument
in the **Range** header like so::

       Range: limit=20
       Range: offset=200; limit=100

Both *limit* and *offset* are optional arguments in the Range header;
the default for offset is zero and the default for limit is ten.

The offset value can be any number - the API will return a default
limit of ten results from that offset. The response will also return a
``Link`` header to reference the Previous, Current, and Next positions
based on the offset you've provided.

This API method will return ALL pairs regardless of whether they are
ranking or unranked (not in the SERPs)!! If you wish to return a
result of *only* the ranking or unranked pairs, see below for the
proper methods.

Each pair item contains the unique ID of the item (used to retrieve
its timeline, to update it, or to delete it), the configured URL and
keyword, as well as the datetime created and datetime modified (as a
Unix epoch timestamp).

This method includes the pair's settings in the resulting output.

Here is an example in Python::

       ...
       # Build the request
       req = urllib2.Request('https://secure.whooshtraffic.com/ranktracker/pairs')
       req.add_header('Authorization', 'Basic %s' % auth)
       req.add_header('Accept', 'application/vnd.whoosh.api+json')
       req.add_header('Range',  'offset=0')
       
       # Assuming the SSL verifying opener
       result  = opener.open(req).read()
       decoded = json.loads(result)

The JSON result::

       [{"url": "http://yourdomain.com", "modified": 1321384422, "id": 18735, "keyword": "your keyword1", "locale" : "98726", "created": 1318965959, "settings" : {"lang" : "en", "country" : "US", "tld" : ".com"}},
        {"url": "http://yourdomain.com", "modified": 1321383485, "id": 18736, "keyword": "your keyword2", "locale" : "98726", "created": 1318965975, "settings" : {"lang" : "en", "country" : "US", "tld" : ".com"}},
        {"url": "http://yourdomain.com", "modified": 1321376887, "id": 18737, "keyword": "your keyword3", "locale" : "98726", "created": 1318965988, "settings" : {"lang" : "en", "country" : "US", "tld" : ".com"}},
        {"url": "http://yourdomain.com", "modified": 1321381710, "id": 18738, "keyword": "your keyword4", "locale" : "98726", "created": 1318966006, "settings" : {"lang" : "en", "country" : "US", "tld" : ".com"}},
        {"url": "http://yourdomain.com", "modified": 1321381018, "id": 18739, "keyword": "your keyword5", "locale" : "98726", "created": 1318966024, "settings" : {"lang" : "en", "country" : "US", "tld" : ".com"}},
        {"url": "http://yourdomain.com", "modified": 1321341601, "id": 20204, "keyword": "your keyword6", "locale" : "98726", "created": 1319296358, "settings" : {"lang" : "en", "country" : "US", "tld" : ".com"}},
        {"url": "http://yourdomain.com", "modified": 1321341601, "id": 20205, "keyword": "your keyword7", "locale" : "98726", "created": 1319296383, "settings" : {"lang" : "en", "country" : "US", "tld" : ".com"}},
        {"url": "http://yourdomain.com", "modified": 1321383132, "id": 20206, "keyword": "your keyword8", "locale" : "98726", "created": 1319296396, "settings" : {"lang" : "en", "country" : "US", "tld" : ".com"}},
        {"url": "http://yourdomain.com", "modified": 1321372078, "id": 20207, "keyword": "your keyword9", "locale" : "98726", "created": 1319296411, "settings" : {"lang" : "en", "country" : "US", "tld" : ".com"}},
        {"url": "http://yourdomain.com", "modified": 1321385958, "id": 20208, "keyword": "your keyword10", "locale" : "98726", "created": 1319296425, "settings" : {"lang" : "en", "country" : "US", "tld" : ".com"}}]

==============================================================
GET all (or a range of) configured *ranking* URL/Keyword pairs
==============================================================

The endpoint path for this API method is:
``https://secure.whooshtraffic.com/ranktracker/pairs/ranked``

The result set will be all of the URL/Keyword pairs that the rank
tracker is actively tracking in the SERPs.

You may also provide an engine argument in the ``Range`` header, the
only two search engines supported at this time are Google and Bing::

       Range: engine=google

If no engine argument is supplied we will return pairs that are
ranking for BOTH Bing and Google

The result set will not specify the engine, the programmer must keep
track of this in their application when passing the engine argument in
the ``Range`` header. This is because we have only one pair and search
both engines based on the pair, then store which result is matched to
which engine in the results - so we don't have any correlation between
the two (except when returning results as is the case with the
GET_timeline method).

This method includes the pair's settings in the resulting output.

The header arguments and output of this API method is precisely the
same as that of the generic GET ALL above. Refer to the section above
for requirements regarding the ``Accept`` and ``Range`` headers.

===============================================================
GET all (or a range of) configured *unranked* URL/Keyword pairs
===============================================================

The endpoint path for this API method is:
``https://secure.whooshtraffic.com/ranktracker/pairs/unranked``

The result set will be all of the URL/Keyword pairs that the rank
tracker has either not found at all or has been unable to any longer
find in the SERPs.

You may also provide an engine argument in the ``Range`` header, the
only two search engines supported at this time are Google and Bing::

       Range: engine=google

If no engine argument is supplied we will return pairs that are
not ranking for BOTH Bing and Google

The result set will not specify the engine, the programmer must keep
track of this in their application when passing the engine argument in
the ``Range`` header. This is because we have only one pair and search
both engines based on the pair, then store which result is matched to
which engine in the results - so we don't have any correlation between
the two (except when returning results as is the case with the
GET_timeline method).

This method includes the pair's settings in the resulting output.

The header arguments and output of this API method is precisely the
same as that of the generic GET ALL above. Refer to the section above
for requirements regarding the ``Accept`` and ``Range`` headers.

=====================
GET a pair's timeline
=====================

The endpoint path for this API method is:
``https://secure.whooshtraffic.com/ranktracker/pairs/{id}/timeline``

``{id}`` is the unique ID of the pair retreived from either a GET all or
GET one request.

If you DO NOT specify a ``Range`` header this method will default to
returning one month's worth of timeline data from the present date
(UTC) for the pair!

If you do specify a ``Range`` header it must be supplied in this
format::

       Range: months=3

The months value can be any number between 0 and 9999 - the API will
return timeline data in that date range from the present date (UTC).

You may also provide an engine argument in the ``Range`` header, the
only two search engines supported at this time are Google and Bing::

       Range: engine=google

Specifying both arguments requires a semicolon separating them::

       Range: months=3; engine=google

If no engine argument is supplied BOTH Bing and Google results will be
returned in the result set!

The server will return a list of objects (in XML or JSON depending on
your Accept header); each of which contains the timestamp (UTC) the
rank was checked, the change in rank from the previous check, and the
rank. The change field will be either 0 (no change in rank), a
positive integer representing an increase in rank, or a negative
integer representing a decrease in rank.

It is important to note that a positive increase in rank will mean the
rank number is approaching one as it increases and a negative decrease
in rank will indicate the rank is moving further from one.

If you do not have rights to the pair ID, if it does not exist, OR if
the specified pair does not have a timeline (it is unranked) it will
return a 404 error code.

An example in Python::

       ...
       # Build the request
       req = urllib2.Request('https://secure.whooshtraffic.com/ranktracker/pairs/234/timeline')
       req.add_header('Authorization', 'Basic %s' % auth)
       req.add_header('Accept', 'application/vnd.whoosh.api+json')
       req.add_header('Range', 'months=2; engine=google')
       
       # Assuming the SSL verifying opener
       result  = opener.open(req).read()
       decoded = json.loads(result)

The JSON result::

       # Note you may not have 2 month's worth of results
       [{"timestamp": 1319278514, "engine": "google",  "change": 0, "rank": 405},
        {"timestamp": 1319293812, "engine": "google",  "change": -29, "rank": 434},
        {"timestamp": 1319377261, "engine": "google",  "change": -92, "rank": 526},
        {"timestamp": 1319464226, "engine": "google",  "change": -11, "rank": 537},
        {"timestamp": 1319800474, "engine": "google",  "change": 471, "rank": 66},
        {"timestamp": 1319896358, "engine": "google",  "change": 2, "rank": 64},
        {"timestamp": 1319984607, "engine": "google",  "change": -3, "rank": 67},
        {"timestamp": 1320071539, "engine": "google",  "change": 1, "rank": 66},
        {"timestamp": 1320159986, "engine": "google",  "change": -1, "rank": 67},
        {"timestamp": 1320246062, "engine": "google",  "change": 6, "rank": 61},
        {"timestamp": 1320334686, "engine": "google",  "change": -3, "rank": 64},
        {"timestamp": 1320420335, "engine": "google",  "change": 2, "rank": 62},
        {"timestamp": 1320505928, "engine": "google",  "change": 22, "rank": 40},
        {"timestamp": 1320623904, "engine": "google",  "change": -1, "rank": 41},
        {"timestamp": 1320684386, "engine": "google",  "change": 0, "rank": 41},
        {"timestamp": 1320767756, "engine": "google",  "change": 12, "rank": 29},
        {"timestamp": 1320767758, "engine": "google",  "change": 0, "rank": 5},
        {"timestamp": 1320862465, "engine": "google",  "change": 0, "rank": 29},
        {"timestamp": 1320968296, "engine": "google",  "change": 0, "rank": 29},
        {"timestamp": 1320968298, "engine": "google",  "change": 0, "rank": 5},
        {"timestamp": 1321037673, "engine": "google",  "change": 1, "rank": 28},
        {"timestamp": 1321037674, "engine": "google",  "change": 1, "rank": 4},
        {"timestamp": 1321124212, "engine": "google",  "change": 0, "rank": 4},
        {"timestamp": 1321223416, "engine": "google",  "change": 0, "rank": 4}]

=====================
GET a pair's settings
=====================

The endpoint path for this API method is:
``https://secure.whooshtraffic.com/ranktracker/pairs/{id}/settings``

``{id}`` is the unique ID of the pair retrieved from either a GET all or
GET one request.

The server will return a dictionary with the configured ISO2 country
code, ISO2 language code, and the configured TLD.

An example in Python::

       ...
       # Build the request
       req = urllib2.Request('https://secure.whooshtraffic.com/ranktracker/pairs/234/settings')
       req.add_header('Authorization', 'Basic %s' % auth)
       req.add_header('Accept', 'application/vnd.whoosh.api+json')
       
       # Assuming the SSL verifying opener
       result  = opener.open(req).read()
       decoded = json.loads(result)

The JSON result::

       {"lang": "ja", "country": "US", "tld": ".com"}

=============================
GET a count of all your pairs
=============================

The endpoint path for this API method is:
``https://secure.whooshtraffic.com/ranktracker/pairs/count``

This method will return a count of all your pairs.

The server will return a document with the value in the body as
plaintext - it will not return it as a json encoded string or xml. It
can, at all times, be safely coerced to an integer value.

An example in Python::

       ...
       # Build the request
       req = urllib2.Request('https://secure.whooshtraffic.com/ranktracker/pairs/count')
       req.add_header('Authorization', 'Basic %s' % auth)
       
       # Assuming the SSL verifying opener
       result  = opener.open(req).read()

=====================================
GET a count of all your ranking pairs
=====================================

The endpoint path for this API method is:
``https://secure.whooshtraffic.com/ranktracker/pairs/ranked_count``

This method will return a count of all your ranking pairs.

The server will return a document with the value in the body as
plaintext - it will not return it as a json encoded string or xml. It
can, at all times, be safely coerced to an integer value.

An example in Python::

       ...
       # Build the request
       req = urllib2.Request('https://secure.whooshtraffic.com/ranktracker/pairs/ranked_count')
       req.add_header('Authorization', 'Basic %s' % auth)
       
       # Assuming the SSL verifying opener
       result  = opener.open(req).read()

=======================================
GET a count of all your unranking pairs
=======================================

The endpoint path for this API method is:
``https://secure.whooshtraffic.com/ranktracker/pairs/unranked_count``

This method will return a count of all your unranking pairs.

The server will return a document with the value in the body as
plaintext - it will not return it as a json encoded string or xml. It
can, at all times, be safely coerced to an integer value.

An example in Python::

       ...
       # Build the request
       req = urllib2.Request('https://secure.whooshtraffic.com/ranktracker/pairs/unranked_count')
       req.add_header('Authorization', 'Basic %s' % auth)
       
       # Assuming the SSL verifying opener
       result  = opener.open(req).read()

==========================================
GET a pair's latest page cache of the rank
==========================================

The endpoint path for this API method is:
``https://secure.whooshtraffic.com/ranktracker/pairs/{id}/cache``

``{id}`` is the unique ID of the pair retreived from either a GET all or
GET one request.

The server will return the document body as-is. If there is no rank
(therefore no page-cache) this method will return a 404.

An example in Python::

       ...
       # Build the request
       req = urllib2.Request('https://secure.whooshtraffic.com/ranktracker/pairs/234/cache')
       req.add_header('Authorization', 'Basic %s' % auth)
       
       # Assuming the SSL verifying opener
       # This also returns straight HTML (no json)
       result  = opener.open(req).read()

The document will be a valid HTML document scraped from the Google
SERP page.

================================
GET a checksum of all your pairs
================================

The endpoint path for this API method is:
``https://secure.whooshtraffic.com/ranktracker/pairs/checksum``

This method retrieves all of your pairs just as it does with ``GET all``
above, converts it to a json string and hashes that string with md5 to
produce the checksum.

If you are caching the result set provided by ``GET all``, you can
request a checksum from the server and compute your own checksum from
the cached pairs that you have.

When you compute your own checksum, ensure your cached pairs is a json
string (exactly as the API would return it to you).

The server will return a document with the checksum in the body - it
will not return it as a json encoded string or xml.

An example in Python::

       ...
       # Build the request
       req = urllib2.Request('https://secure.whooshtraffic.com/ranktracker/pairs/checksum')
       req.add_header('Authorization', 'Basic %s' % auth)
       
       # Assuming the SSL verifying opener
       result  = opener.open(req).read()

================================
GET a checksum for a single pair
================================

The endpoint path for this API method is:
``https://secure.whooshtraffic.com/ranktracker/pairs/{id}/checksum``

This method retrieves a single pair, just as it does with ``GET one``
above, converts it to a json string and hashes that string with md5 to
produce the checksum.

If you are caching the result set provided by ``GET one``, you can
request a checksum from the server and compute your own checksum from
the cached pair that you have.

When you compute your own checksum, ensure your cached pair is a json
string (exactly as the API would return it to you).

The server will return a document with the checksum in the body - it
will not return it as a json encoded string or xml.

An example in Python::

       ...
       # Build the request
       req = urllib2.Request('https://secure.whooshtraffic.com/ranktracker/pairs/91512/checksum')
       req.add_header('Authorization', 'Basic %s' % auth)
       
       # Assuming the SSL verifying opener
       result  = opener.open(req).read()

===================================
GET a checksum for all ranked pairs
===================================

The endpoint path for this API method is:
``https://secure.whooshtraffic.com/ranktracker/pairs/ranked_checksum``

This method retrieves all of your ranking pairs just as it does with
``GET all ranked`` above, converts it to a json string and hashes that string
with md5 to produce the checksum.

If you are caching the result set provided by ``GET all ranked``, you
can request a checksum from the server and compute your own checksum
from the cached pairs that you have.

When you compute your own checksum, ensure your cached pairs is a json
string (exactly as the API would return it to you).

The server will return a document with the checksum in the body - it
will not return it as a json encoded string or xml.

An example in Python::

       ...
       # Build the request
       req = urllib2.Request('https://secure.whooshtraffic.com/ranktracker/pairs/ranked_checksum')
       req.add_header('Authorization', 'Basic %s' % auth)
       
       # Assuming the SSL verifying opener
       result  = opener.open(req).read()

=====================================
GET a checksum for all unranked pairs
=====================================

The endpoint path for this API method is:
``https://secure.whooshtraffic.com/ranktracker/pairs/unranked_checksum``

This method retrieves all of your unranking pairs just as it does with
``GET all unranked`` above, converts it to a json string and hashes
that string with md5 to produce the checksum.

If you are caching the result set provided by ``GET all unranked``,
you can request a checksum from the server and compute your own
checksum from the cached pairs that you have.

When you compute your own checksum, ensure your cached pairs is a json
string (exactly as the API would return it to you).

The server will return a document with the checksum in the body - it
will not return it as a json encoded string or xml.

An example in Python::

       ...
       # Build the request
       req = urllib2.Request('https://secure.whooshtraffic.com/ranktracker/pairs/unranked_checksum')
       req.add_header('Authorization', 'Basic %s' % auth)
       
       # Assuming the SSL verifying opener
       result  = opener.open(req).read()

======================
GET your account quota
======================

The endpoint path for this API method is:
``https://secure.whooshtraffic.com/ranktracker/pairs/quota``

It returns a JSON object containing your quota and the number of pairs
you've used.


