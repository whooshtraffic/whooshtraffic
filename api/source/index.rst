.. WhooshTraffic documentation master file, created by
   sphinx-quickstart on Thu Apr 28 15:23:40 2011.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

===
API
===

Contents:

.. toctree::
   :maxdepth: 2
   
   API <api>

===================
A Basic PHP Example
===================

This example is meant to complement the Python based examples, please
refer to the `PHP cURL Docs <http://php.net/manual/en/book.curl.php>`_
and `PHP JSON Docs <http://php.net/manual/en/book.json.php>`_ for
further explanation of cURL options.

This PHP example will make use of an object oriented cURL wrapper
library Parnell wrote for the opensource KohanaPHP framework:

* As a ZIP archive: `curl.zip <https://secure.whooshtraffic.com/static/downloads/curl.zip>`_
* As a PAX archive: `curl.pax <https://secure.whooshtraffic.com/static/downloads/curl.pax>`_

The first thing to keep in mind is that PHP's cURL implementation will
already **base64** encode your authorization header value, please use
the example below as a reference for your own implementation::

       <?php
       
       require 'Curl.php';
       
       # Set your login token and key
       $login = "dfjsgIDKsHOskd32dsfw";
       $key   = "ksdfjsjf922j2ksdkf.laksdjfkjaj23dskf/fjjsd.dfss/s7O";
       
       # Setup initial cURL options
       $options = array
       (
           CURLOPT_FAILONERROR      => False,
           CURLOPT_FOLLOWLOCATION   => True,
           CURLOPT_RETURNTRANSFER   => True,
           CURLOPT_FRESH_CONNECT    => True,
           CURLOPT_FORBID_REUSE     => True,
           CURLOPT_POST             => False,
           CURLOPT_URL              => "https://secure.whooshtraffic.com/ranktracker/pairs",
           CURLOPT_HTTPAUTH         => CURLAUTH_BASIC,
           CURLOPT_USERPWD          => $login.":".$key, # cURL base64 encodes it automatically
           CURLOPT_HTTPHEADER       => array("Accept: application/vnd.whoosh.api+json", "Range: offset=0"),
           CURLOPT_SSL_VERIFYHOST   => 2
       );
       
       $curl   = new Curl($options);
       
       # Execute the cURL session and ignore cURL errorno "22" so we can branch on HTTP status codes
       $result = $curl->execute(array(22));
       
       # Return the status, this defaults to the HTTP status code returned by the session
       echo $curl->status() . "<br />";
       
       if ($curl->status() === 200)
       {
           echo var_export(json_decode($result));
       } else {
           echo $result;
       }

==================
Indices and tables
==================

* :ref:`genindex`
* :ref:`search`

