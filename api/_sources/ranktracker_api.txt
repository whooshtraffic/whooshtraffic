Rank Tracker API Documentation
==============================

All of these examples use the custom Python urllib2 opener that
validates the server's ssl certificate against a local PEM bundle
:doc:`api`. It is highly recommended that you validate the server's
certificates. Examples in other languages (Ruby, PHP, etc...)  can be
easily found on the internet and in the documentation of the
respective languages.

In addition to the authorization header, the API requires you to
specify (exception being PUT, POST, and DELETE) the content type you
wish to accept; the Whoosh Traffic API only supports returning XML or
JSON. The Accept headers for these two content types are vendor
specific, these are the strings you would use (see examples too)::

       Accept: application/vnd.whoosh.api+json

or::

       Accept: application/vnd.whoosh.api+xml

.. toctree::
   :maxdepth: 2
   
   GET Requests <ranktracker_api_get>
   HEAD Requests <ranktracker_api_head>
   POST Requests <ranktracker_api_post>
   PUT Requests <ranktracker_api_put>
   DELETE requests <ranktracker_api_delete>
