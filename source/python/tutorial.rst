
Tutorial
========

Installation
------------------

To get started with the Python library for Shodan, first make sure that you've
`received your API key <http://www.shodanhq.com/api_doc>`_. Once that's done,
install the library via the cheeseshop using:

.. code-block:: bash
	
	$ easy_install shodan

Or if you already have it installed and want to upgrade to the latest version:

.. code-block:: bash
	
	$ easy_install -U shodan

It's always safe to update your library as backwards-compatibility is preserved.
Usually a new version of the library simply means there are new methods/ features
available.


Getting started
---------------

The first thing we need to do in our code is to initialize the API object:

.. code-block:: python

	from shodan import WebAPI
	
	SHODAN_API_KEY = "insert your API key here"
	
	api = WebAPI(SHODAN_API_KEY)

	
Searching Shodan
----------------

Now that we have our API object all good to go, we're ready to perform a search:

.. code-block:: python
	
	# Wrap the request in a try/ except block to catch errors
	try:
		# Search Shodan
		results = api.search('apache')
		
		# Show the results
		print 'Results found: %s' % results['total']
		for result in results['matches']:
			print 'IP: %s' % result['ip']
			print result['data']
			print ''
	except Exception, e:
		print 'Error: %s' % e

Stepping through the code, we first call the :py:func:`WebAPI.search` method on the `api` object which
returns a dictionary of result information. We then print how many results were found in total,
and finally loop through the returned matches and print their IP and banner.

There's a lot more information that gets returned by the function. See below for a shortened example
dictionary that :py:func:`WebAPI.search` returns:

.. code-block:: python
	
	{
		'total': 8669969,
		'countries': [
			{
				'code': 'US',
				'count': 4165703,
				'name': 'United States'
			},
			{'code': 'DE', 'count': 610270, 'name': 'Germany'},
			{'code': 'JP', 'count': 496556, 'name': 'Japan'},
			{'code': 'RO', 'count': 486107, 'name': 'Romania'},
			{'code': 'GB', 'count': 273948, 'name': 'United Kingdom'}
		],
		'matches': [
			{
				'country': 'DE',
				'data': 'HTTP/1.0 200 OK\r\nDate: Mon, 08 Nov 2010 05:09:59 GMT\r\nSer...',
				'hostnames': ['pl4t1n.de'],
				'ip': '89.110.147.239',
				'os': 'FreeBSD 4.4',
				'port': 80,
				'updated': '08.11.2010'
			},
			...
		]
	}

It's also good practice to wrap all API requests in a try/ except clause, since any error
will raise an exception. But for simplicity's sake, I will leave that part out from now on.

Looking up a host
-----------------

To see what Shodan has available on a specific IP we can use the :py:func:`WebAPI.host` function:

.. code-block:: python
	
	# Lookup the host
	host = api.host('217.140.75.46')
	
	# Print general info
	print """
		IP: %s
		Country: %s
		City: %s
	""" % (host['ip'], host.get('country', None), host.get('city', None))
	
	# Print all banners
	for item in host['data']:
		print """
			Port: %s
			Banner: %s
			
		""" % (item['port'], item['banner'])