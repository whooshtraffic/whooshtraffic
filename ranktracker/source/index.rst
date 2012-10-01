.. Whoosh Traffic! Ranktracker documentation master file, created by
   sphinx-quickstart on Mon Jan 23 10:40:59 2012.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

The Whoosh Traffic! Ranktracker
===============================

Contents:

.. toctree::
   :maxdepth: 2
   
   Caveats and Known Issues <caveats>
   Filters <filters>
   Reports <reports>
   Settings <settings>

.. admonition:: Looking for the...
   
   `Ranktracker API documentation <https://secure.whooshtraffic.com/static/documentation/api/>`_?

How it Works
============

The Whoosh Traffic! Ranktracker tracks a configured URL/keyword pair
in a search engine's index by submitting a search query to that
engine's search page just as a user would (using any advanced
settings) and scanning each page of results until a match is found,
the maximum depth in the results for the account is reached, or the
end of the results is reached.

.. note::
   The Ranktracker only supports Google and Bing. Yahoo! search has been subsidized by Bing.

If a match is found, we store its rank position and calculate its
change from the previous known rank (if it has one). The rank and
change are calculated on a "nearing-one" scale as the first position
in the SERP set is also the "highest" rank.

When a URL/keyword pair is first created the ranktracker will do two
things:

1. Check to see if it is even in the index, if it is the rank is stored.

  * If not, and there is a "www" prefixing the URL it will be stripped and re-tracked; if a match is found with the stripped version then a **new** URL/keyword pair is created in place of that one and a reference to the new one is made. *This same process is repeated for a URL that isn't ranking but has no "www" prefixing it, in this case a "www" prefixed version is checked for.*

2. If the URL is a *domain only* ie: ``http://mydomain.com`` instead of: ``http://mydomain.com/some/path`` URL, then the ranktracker will scan for any deep links containing that exact domain (if there is a subdomain, that is considered part of the domain!) in the SERPs for the given keyword.

After the initial track (1) and scan (2) - the pair will be queued in the daily tracking schedule and the weekly scanning schedule. The daily tracker runs once a day at midnight and the weekly scanner runs at midnight on Sunday.

The scanner automatically adds in URL's it finds so the tracker will track them in the daily schedule. This, however, can lead to result pollution - when you delete any URL/keyword pair from the interface it will also mark it as "no_scan" which means that if the URL was added by the scanner and you deleted it, the scanner will never add that URL again.
