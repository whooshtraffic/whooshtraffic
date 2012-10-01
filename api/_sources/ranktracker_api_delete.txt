==================================
DELETE removing a URL/Keyword pair
==================================

The endpoint path for the DELETE method is
``https://secure.whooshtraffic.com/ranktracker/pairs/{id}``

``{id}`` is the unique ID retreived from a GET all or GET one
operation. If the item doesn't exist or doesn't belong to you, the
server will return a ``404 Not Found`` error code. This API method
only supports deleting one item at a time.

If the DELETE request succeeds, the server will return a ``204 No
Content`` response with a zero length body.

An example in Python::

       # We've already setup an SSL verifying opener
       ...
       # Assuming these to be a valid login/key
       login = "Rm7YCcTm12rHcrQ"
       key   = "UKBWgXO1YPPItM5Rs8vq1uO5g2YKcvlqQO9n9XtBF1AE1rRofPUk2"
       
       # Format and b64encode according to 14.8
       auth = base64.b64encode(login+':'+key)
       
       # Build the request
       req = urllib2.Request('https://secure.whooshtraffic.com/ranktracker/pairs/938')
       req.headers['Authorization']  = 'Basic %s' % auth
       req.get_method = lambda: 'DELETE'
       
       # Assuming the SSL verifying opener
       result  = opener.open(req)
