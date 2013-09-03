
Tutorial
========

Installation
------------------

To get started with the Ruby library for Shodan, first make sure that you've
`received your API key <http://www.shodanhq.com/api_doc>`_. Once that's done,
install the library via rubygems:

.. code-block:: bash
	
	$ gem install shodan

Or if you already have it installed and want to upgrade to the latest version:

.. code-block:: bash
	
	$ gem update shodan

It's always safe to update your library as backwards-compatibility is preserved.
Usually a new version of the library simply means there are new methods/ features
available.


Getting started
---------------

The first thing we need to do in our code is to initialize the API object:

.. code-block:: ruby

	require 'shodan'
	
	SHODAN_API_KEY = "insert your API key here"
	
	api = Shodan::WebAPI.new(SHODAN_API_KEY)

	
Searching Shodan
----------------

Now that we have our API object all good to go, we're ready to perform a search:

.. code-block:: ruby
	
	# Wrap the request in a try/ except block to catch errors
	begin
		# Search Shodan
		results = api.search('apache')
		
		# Show the results
		puts "Results found: #{results['total']}"
		
		results['matches'].each { |result|
			puts "IP: #{result['ip']}"
			puts result['data']
		}
	rescue Exception => e
		puts "Error: #{e.to_s}"
	end

Stepping through the code, we first call the :rb:meth:`Shodan::WebAPI#search` method on the `api` object which
returns a hash of result information. We then print how many results were found in total,
and finally loop through the returned matches and print their IP and banner.

There's a lot more information that gets returned by the function. See below for a shortened example
hash that :rb:meth:`Shodan::WebAPI#search` returns:

.. code-block:: ruby
	
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

It's also good practice to wrap all API requests in a begin/ rescue clause, since any error
will raise an exception. But for simplicity's sake, I will leave that part out from now on.

Looking up a host
-----------------

To see what Shodan has available on a specific IP we can use the :rb:meth:`Shodan::WebAPI#host` function:

.. code-block:: ruby
	
	# Lookup the host
	host = api.host('217.140.75.46')
	
	# Print general info
	puts <<INFO
		IP: #{host['ip']}
		Country: #{host['country']}
		City: #{host['city']}
	INFO
	
	# Print all banners
	host['data'].each { |item|
		puts "Port: #{item['port']}"
		puts item['banner']
	}
