
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

Using the Exploit DB
--------------------

The `Exploit DB <http://www.exploit-db.com>`_ website provides an archive of known exploits for
various applications. Using the Shodan API it's very easy now to access their archive from
within your program. Currently, there are 2 relevant functions: :py:func:`WebAPI.exploitdb.search`
and :py:func:`WebAPI.exploitdb.download`.

.. code-block:: python
	
	# Search Exploit DB
	results = api.exploitdb.search('PHP')
	
	# Print result information
	print 'Results found: %s' % results['total']
	for exploit in results['matches']:
		print '%s: %s' % (exploit['id'], exploit['description'])

The Exploit DB :py:func:`WebAPI.exploitdb.search` function returns a dictionary very similar to
Shodan's :py:func:`WebAPI.search` method. Below is an example of what it looks like:

.. code-block:: python

	{
		'total': 2192,
		'matches': [
			{
				'author': 'MP',
				'cve': '2010-3709',
				'date': '18.10.2006',
				'description': 'Php AMX 0.90 (plugins/main.php) Remote File Include Vulnerability',
				'id': 2591,
				'platform': 'php',
				'port': 0,
				'type': 'webapps'
			},
			...
		]
	}

To download the actual exploit, we provide the exploit ID to the :py:func:`WebAPI.exploitdb.download`
function:

.. code-block:: python
	
	# Download the file
	file = api.exploitdb.download(exploit['id'])
	
	# Show the contents
	print 'Filename: %s' % file['filename']
	print 'Content-type: %s' % file['content-type']
	print file['data']

The resulting dictionary has 3 fields: `content-type`, `filename` and `data`.

Searching Metasploit Modules
----------------------------

The `Metasploit framework <http://www.metasploit.com>`_ is a highly popular and versatile platform
for penetration testing and security development. It offers more than a thousand modules that
contain a variety of functions. Using the Shodan API it's now possible to search the archive
of Metasploit modules, get information about them and even download their source code.

The 2 new functions are: :py:func:`WebAPI.msf.search` and :py:func:`WebAPI.msf.download`.
Lets take a look at the :py:func:`WebAPI.msf.search` first:

.. code-block:: python
	
	# Search for a Metasploit module
	modules = api.msf.search('microsoft')
	
	# Print basic information about the module
	print 'Modules found: %s' % modules['total']
	for module in results['matches']:
		print '%s: %s' % (module['type'], module['name'])

The information contained in the `module` hash is identical to what you find in the Metasploit documentation.
It contains everything from the module description, type, rank, references and more. It's a long list, I suggest
printing out the entire dictionary to see what's in it. This is an alternate for loop that does that, but it's not
as nicely formated as the version above.

.. code-block:: python
	
	for module in results['matches']:
		print module

Once you've found the module you were looking for, use the :py:func:`WebAPI.msf.download` function to
download the actual module code and see what it looks like:

.. code-block:: python
	
	# Download the file
	file = api.msf.download(module['fullname'])
	
	# Show the contents
	print 'Filename: %s' % file['filename']
	print 'Content-type: %s' % file['content-type']
	print file['data']

The only argument you need to pass the function is the `fullname` of the module. I hope this has given you some
ideas on integrating Metasploit module information in your program.

.. Using the Dataloss DB
.. ---------------------
.. 
.. The `Dataloss DB <http://www.datalossdb.org>`_ website provides an archive of incidents/ data losses of
.. various companies and organizations. Using the Shodan API it's very easy now to access their archive from
.. within your program. Currently, there's 1 function for searching the archive: :py:func:`WebAPI.dataloss.search`.
.. 
.. .. code-block:: python
.. 	
.. 	# Search Dataloss DB archive for incidents related to Sony
.. 	results = api.dataloss.search(name='Sony')
.. 	
.. 	# Print result information
.. 	print 'Results found: %s' % results['total']
.. 	for incident in results['matches']:
.. 		print 'Name: %s' % incident['name']
.. 		print 'Records: %s' % incident['records']
.. 
.. The Dataloss DB :py:func:`WebAPI.dataloss.search` function returns a dictionary very similar to
.. Shodan's :py:func:`WebAPI.search` method. Below is an example of what it looks like:
.. 
.. .. code-block:: python
.. 
.. 	{
.. 		'matches': [
.. 			{
.. 				'arrest': False,
..                 'breaches': ['Hack'],
..                 'country':  'US',
..                 'data': ['CCN'],
..                 'date': '10.01.2000',
..                 'ext': False,
..                 'ext_names': [''],
..                 'lawsuit': False,
..                 'link': 'http://datalossdb.org/incidents/6',
..                 'name': 'CD Universe',
..                 'records': 300000,
..                 'recovered': False,
..                 'source': 'Outside',
..                 'stocks': [''],
..                 'sub_types': ['Retail'],
..                 'types': ['Biz'],
..                 'uid': 6
..             }
..         ],
..  		'total': 1
..  	}

WiFi Positioning System
-----------------------

Popularized by Samy Kamkar's "How I met your girlfriend" talk, it's possible to look up the physical address
of a wireless router using services such as Google Locations and Skyhook. At the time of this writing,
neither of those websites provide an official web service to access that information. For that reason
there's now an unofficial way to access that information using Shodan's 'wps' module (WiFi Positioning System).
Here's how you use it:

.. code-block:: python
	
	#  Import the class
	from shodan.wps import GoogleLocation
	
	# Setup the object
	wps_client = GoogleLocation()
	
	# Locate the physical address of the MAC/ BSSID
	address = wps_client.locate('00:1D:7E:F0:A2:B0')
	
The `address` variable at that point contains a `dictionary` of variables that contain all the information.
If the accuracy field is 42000, that means no results were found. Here's how a successful result looks like:

.. code-block:: python
	
	{
		'access_token': '2:rbShPJe-4LAay7E2:JC1CZE44kD144m5H',
 		'location': {
 			'accuracy': 24.0,
			'address': {'city': 'Portland',
				'country': 'United States',
				'country_code': 'US',
				'county': 'Multnomah',
				'postal_code': '97202',
				'region': 'Oregon',
				'street': 'SE Tolman St',
				'street_number': '1700-1798'},
				'latitude': 45.4773572,
				'longitude': -122.6477922
		}
	}
