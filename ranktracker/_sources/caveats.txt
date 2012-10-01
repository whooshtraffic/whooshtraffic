Caveats and Known Issues
========================

Once in a while, you may notice discrepancies in your ranktracker
results, these discrepancies are caused by a few factors. Here are the
predominant factors:

#. Personalization based on search history, whether you're logged in, &c...
#. Datacenter geo-location and proximity to the requesting IP
#. Geographical location of the requesting IP

Number (1) we can fix by making the search request "anonymous" but
that only helps with one aspect of it. The other two (2)(3) factors
are very difficult to resolve and only have one possible solution - a
solution that requires the user to have their own servers (or to rent
them from us) in the area that they are in, or their clients are in
(to mimic the requests coming from the same location to defeat the
geolocation and proximity issues). We have not implemented this yet
but may do so if demand for it increases.

.. admonition:: Please note...
   
   That when you are logged in, personalization is "turned on" which
   means you will definitely see discrepancies - what you want to do
   is log out of Google so that you can see what the results look like
   without your search history, ad click history, preferences,
   &c... being factored in by Google.

What kind of discrepancies are we talking about?
------------------------------------------------

In some mild cases, during a search you will see your URL a few
positions different from what the Ranktracker sees.

In less mild cases your URL will be on different pages (either
increasing in rank, or decreasing).

Extreme cases are those in which the URL is ranking for you when you
search and *not ranking at all* for the results the Ranktracker
sees. This usually is a result of both geolocation and the index that
the specific data center has lagging behind (or it might just entirely
be a geolocation issue).

What to do about it?
--------------------

There isn't really much you can do about it - but for those that have
discrepancies, use the results as a trending tool if you aren't
getting exact results.

If you wish to double check what the Rank Tracker is seeing, you can
click on the "Details" button to the right on the row of the pair you
wish to inspect. On that page you will see a graph and to the right of
the graph is a button named "Last Rank Cache", clicking that will give
you the stored page cache we have that the rank tracker saw when it
tracked your item.

How Google Search works
-----------------------

Google search results are *unique*.

The main reason is that there are many variables that Google looks at when returning your results. It uses past searches, region, your browser type, what ads you've clicked on, where you shop, `third-party accounts <https://accounts.google.com/b/0/IssuedAuthSubTokens?hl=en>`_ you've authorized, browsing history and *most interesting* - `inferred interests and demographics <https://www.google.com/settings/ads/onweb/?sig=ACi0TCg3YhvuNCyJMHsR2HOYROFG21bx3TJpQJus9VMbcuMUSjvhILaBccXsjs_GTOg-n64eUmepBwaNJ_erFQPLSfXIP1mFSQeRAT9ppSZHjO0tVpNRaWDTMFa8RBew8CCl9iZbklBvghsmRJLtbPTWIrDtCa8mTdhVMDdU132ayDzSKS-8ZvVxEIL6x3ggUEZm4FsK9eAlNT94WD3ImXrqclUVRW1Low&hl=en>`_. You can test this by performing a search and then signing out of Google, closing the tab and opening a new one to start a new session then doing the search again noting how the results change.

One variable you can't control is which data center is returning the results. There are a few in the US and some internationally. Which one you hit is random and varies with each use. Data centers are consistent with one another but a few are different because they are not using the same algorithm.
