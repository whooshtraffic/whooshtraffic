===========================================
POST adding a URL/Keyword pair for tracking
===========================================

The endpoint path for the POST method is
``https://secure.whooshtraffic.com/ranktracker/pairs``

This API method only supports adding one item at a time.

POST requests must be accompanied by an ``Accept`` header telling the
server what format you want any possible error messages and/or the
results returned as::

       Accept: application/vnd.whoosh.api+json

or::

       Accept: application/vnd.whoosh.api+xml

It requires an XML document of this form::

       <?xml version="1.0" encoding="UTF-8"?>
       <pair>
         <url>http://someurl.com</url>
         <keyword>a keyword</keyword>
       </pair>

* The URL field must be a valid URL. If no URL scheme ("http" or "https") is provided, we will assume "http".
* The URL field will also be validated for any HTTP errors, in particular, any of the 30x response codes (except for 302 and 304), 40x level codes, and whether the hostname is known or not.

If you wish to change the locale (spoof the actual cookie location it comes from) you can supply this element::

       <?xml version="1.0" encoding="UTF-8"?>
       <pair>
         <url>http://someurl.com</url>
         <keyword>a keyword</keyword>
         <locale>87505</locale>
       </pair>

.. note::
   The locale value *must be* a five digit zipcode. Only USA postal codes are supported at this time (we are working on international cookie spoofing).

If you do not supply a custome locale value, we will attempt to use the zipcode attached to your billing address. If we do not have a nearest matching value, then no locale will be used at all.

If you wish to change the TLD (google.com, google.co.uk, google.com.hk, &c...), the language, or the country you can specify (all optional) settings - any setting (or all of them) NOT included in the XML body will assume the default. The defaults are as follows:

* Country default is **US**
* Language default is **en**
* TLD default is **.com**

Lists for accepted settings:

* `List of accepted TLD domains <https://secure.whooshtraffic.com/static/documentation/google_tlds.txt>`_
* `List of accepted country ISO2 codes <https://secure.whooshtraffic.com/static/documentation/google_accepted_countries.txt>`_
* `List of accepted language ISO2 codes <https://secure.whooshtraffic.com/static/documentation/google_accepted_languages.txt>`_

*Note: Bing has limited support for international conventions, these specific settings ONLY apply to Google.*

An example of adding a pair with different settings::

       <?xml version="1.0" encoding="UTF-8"?>
       <pair>
         <url>http://someurl.com</url>
         <keyword>a keyword</keyword>
         <country>GB</country>
         <tld>.co.uk</tld>
       </pair>

Any authorization errors will come back with an HTTP response code and a plaintext body. Any errors occurring with your request following authorization will come back as an XML document::

       <?xml version="1.0" encoding="UTF-8"?>
       <error>
         <message>Some error message</message>
       </error>

If the POST request succeeds, the server will return a ``201 Created`` response (a location header with the URI of the pair is also included) with an XML document body containing the unique ID of the pair item created::

       <?xml version="1.0" encoding="UTF-8"?>
       <success>
         <pairid>939</pairid>
       </success>

An example in Python::

       ...
       # Assuming these to be a valid login/key
       login = "Rm7YCcTm12rHcrQ"
       key   = "UKBWgXO1YPPItM5Rs8vq1uO5g2YKcvlqQO9n9XtBF1AE1rRofPUk2"
       
       # Format and b64encode according to 14.8
       auth = base64.b64encode(login+':'+key)
       
       data = """<?xml version="1.0" encoding="UTF-8"?>
       <pair>
         <url>http://ixmat.us</url>
         <keyword>parnell springmeyer</keyword>
       </pair>
       """
       
       # Build the request
       req = urllib2.Request('https://secure.whooshtraffic.com/ranktracker/pairs')
       req.headers['Authorization']  = 'Basic %s' % auth
       req.headers['Accept']         = 'application/vnd.whoosh.api+xml'
       req.headers['Content-Length'] = str(len(data))
       req.data                      = data
       req.get_method = lambda: 'POST'
       
       # Assuming the SSL verifying opener
       result  = opener.open(req).read()
