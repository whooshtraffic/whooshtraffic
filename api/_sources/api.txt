=================
API Documentation
=================

Contents:

.. toctree::
   :maxdepth: 2
   
   Rank Tracker <ranktracker_api>

========
Security
========

The WhooshTraffic backend (which includes the API) is served entirely
over ``https``. Any requests to
``http://secure.whooshtraffic.com/ranktracker/...`` will result in a permanent
redirection to the application being served over ``https``. In such a
case, if you attempt to request anything from the API on ``http``, the
request will be redirected to the primary login page and return the
XHTML content.

Whenever possible, the user agent *should absolutely* check the
validity of the server's certificate to ensure communication is not
being intercepted by a ManInTheMiddle with a self signed certificate.

Python's urllib2 doesn't provide an easy way to configure this,
although the socket library does. Following is an example of a url
opener that will check the validity of the server certificate against
a certificate bundle (PEM format)::

       def buildValidatingOpener(ca_certs):
           """Build an opener with a given certs bundle and validate the ssl connection.
           
           @author: http://atlee.ca/blog/2011/02/10/verifying-https-python
           
           """
           
           class VerifiedHTTPSConnection(httplib.HTTPSConnection):
               def connect(self):
                   # Overrides the version in httplib so that we do certificate verification
                   sock = socket.create_connection((self.host, self.port),
                                                   self.timeout)
                   if self._tunnel_host:
                       self.sock = sock
                       self._tunnel()
        
                   # Wrap the socket using verification with the root certs in trusted_root_certs
                   self.sock = ssl.wrap_socket(sock,
                                               self.key_file,
                                               self.cert_file,
                                               cert_reqs=ssl.CERT_REQUIRED,
                                               ca_certs=ca_certs,
                                               )
        
           # Wraps https connections with ssl certificate verification
           class VerifiedHTTPSHandler(urllib2.HTTPSHandler):
               def __init__(self, connection_class=VerifiedHTTPSConnection):
                   self.specialized_conn_class = connection_class
                   urllib2.HTTPSHandler.__init__(self)
        
               def https_open(self, req):
                   return self.do_open(self.specialized_conn_class, req)
        
           https_handler = VerifiedHTTPSHandler()
           url_opener = urllib2.build_opener(https_handler)
           
           return url_opener
       
       opener = buildValidatingOpener("/path/to/cert-bundle.pem")

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
