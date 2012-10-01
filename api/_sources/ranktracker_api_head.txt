=======================================
HEAD check if a URL/Keyword pair exists
=======================================

This method will tell you if the pair resource exists or not. It
returns a zero-length body but it will return the proper HTTP status
codes.

The endpoint path for this API is
``https://secure.whooshtraffic.com/ranktracker/pairs/{id}``

If a pair exists for the given resource, a 200 OK status code will be
returned.

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
       req.get_method = lambda: 'HEAD'

====================================================
HEAD check if a a URL/Keyword pair's timeline exists
====================================================

This method will tell you if a timeline for the pair resource exists
or not. It returns a zero-length body but it will return the proper
HTTP status codes.

The endpoint path for this API is
``https://secure.whooshtraffic.com/ranktracker/pairs/{id}/timeline``

You may also provide an engine argument in the ``Range`` header, the
only two search engines supported at this time are Google and Bing::

       Range: engine=google

If no engine argument is supplied it will check if a timeline exists
with both engines.

If a timeline exists for the given pair resource, a 200 OK status code
will be returned.

An example in Python::

       ...
       # Assuming these to be a valid login/key
       login = "Rm7YCcTm12rHcrQ"
       key   = "UKBWgXO1YPPItM5Rs8vq1uO5g2YKcvlqQO9n9XtBF1AE1rRofPUk2"
       
       # Format and b64encode according to 14.8
       auth = base64.b64encode(login+':'+key)
       
       # Build the request
       req = urllib2.Request('https://secure.whooshtraffic.com/ranktracker/pairs/935/timeline')
       req.add_header('Authorization', 'Basic %s' % auth)
       req.add_header('Range', 'engine=google')
       req.get_method = lambda: 'HEAD'
