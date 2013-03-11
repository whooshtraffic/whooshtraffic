========================================
PUT editing/replacing a URL/Keyword pair
========================================

The endpoint path for the PUT method is
``https://secure.whooshtraffic.com/ranktracker/pairs/{id}``

``{id}`` is the unique ID retreived from a GET all or GET one
operation. If the item doesn't exist or doesn't belong to you, the
server will return a ``404 Not Found`` error code.This API method only
supports editing one item at a time.

PUT requests must be accompanied by an ``Accept`` header telling the
server what format you want any possible error messages returned as::

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
* Both fields are required, simply submit the XML document with the value you wish to change and the other value un-changed (we don't update the DB unless it changes, but this helps ensure schema consistency with incoming data)

If you wish to change the locale (spoof the actual cookie location it comes from) you can supply this element::

       <?xml version="1.0" encoding="UTF-8"?>
       <pair>
         <url>http://someurl.com</url>
         <keyword>a keyword</keyword>
         <locale>87505</locale>
       </pair>

.. note::
   The locale value *must be* a five digit zipcode. Only USA postal codes are supported at this time (we are working on international cookie spoofing).

If you do not supply a custome locale value the old value you specified or we defaulted to, will remain.

If you wish to change the TLD (google.com, google.co.uk, google.com.hk, &c...), the language, or the country you can specify (all optional) settings - any setting (or all of them) NOT included in the XML body will assume the default. The defaults are as follows:

* Country default is **US**
* Language default is **en**
* TLD default is **.com**

Lists for accepted settings:

* `List of accepted TLD domains <https://secure.whooshtraffic.com/static/documentation/google_tlds.txt>`_
* `List of accepted country ISO2 codes <https://secure.whooshtraffic.com/static/documentation/google_accepted_countries.txt>`_
* `List of accepted language ISO2 codes <https://secure.whooshtraffic.com/static/documentation/google_accepted_languages.txt>`_

*Note: Bing has limited support for international conventions, these specific settings ONLY apply to Google.*

An example of editing a pair to use different settings::

       <?xml version="1.0" encoding="UTF-8"?>
       <pair>
         <url>http://someurl.com</url>
         <keyword>a keyword</keyword>
         <country>GB</country>
         <tld>.co.uk</tld>
       </pair>

*Note: if you only wish to edit the settings and not the URL/Keyword you still must provide the URL and Keyword elements with their values un-changed.*

Any authorization errors will come back with an HTTP response code and a plaintext body. Any errors occurring with your request following authorization will come back as an XML document::

       <?xml version="1.0" encoding="UTF-8"?>
       <error>
         <message>Some error message</message>
       </error>

If the PUT request succeeds, the server will return a ``202 Accepted``
response and a zero length body.

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
       req = urllib2.Request('https://secure.whooshtraffic.com/ranktracker/pairs/938')
       req.headers['Authorization']  = 'Basic %s' % auth
       req.headers['Accept']         = 'application/vnd.whoosh.api+xml'
       req.headers['Content-Length'] = str(len(data))
       req.data                      = data
       req.get_method = lambda: 'PUT'
       
       # Assuming the SSL verifying opener
       result  = opener.open(req)
