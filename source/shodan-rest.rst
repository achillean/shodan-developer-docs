
JSON API Reference
==================

.. http:get:: /api/count
	
	Returns the number of devices that a search query found. Unrestricted usage of all advanced filters.

	:query key: Shodan API key
	:query q: Search query

	**Example Request**:

	.. sourcecode:: http

		GET /api/count?q=apache&key=SHODAN_API_KEY

	**Example Response**:

	.. sourcecode:: javascript

		{
			'total': 150041
		}


.. http:get:: /api/host

	Lookup all available information for a specific IP address.
	
	:query key: Shodan API key
	:query ip: IP address of the devices to get information on

	**Example Request**:

	.. sourcecode:: http

		GET /api/host?ip=1.1.1.1&key=SHODAN_API_KEY

	**Example Response**:

	.. sourcecode:: javascript

		{
			'ip': '1.1.1.1',
			'hostnames': ['testing.com'],
			'country_name': 'Switzerland',
			'country_code': 'CH',
			'city': 'Zurich',
			'latitude': null,
			'longitude': null,
			'os': 'Windows 2000',
			'data': [
				{
					'port': 80,
					'banner':  'HTTP/1.0 401 Unauthorized 
							Date: Fri, 08 Feb 2013 01:03:04 GMT 
							Server: cisco-IOS 
							Connection: close 
							Accept-Ranges: none 
							WWW-Authenticate: Basic realm="level_15_access"',
					'timestamp': '24.01.2012'
				}
			]
		}


.. http:get:: /api/info

	Retrieve information about the current API plan.
	
	:query key: Shodan API key

	**Example Request**:

	.. sourcecode:: http

		GET /api/info?key=SHODAN_API_KEY

	**Example Response**:

	.. sourcecode:: javascript

		{
			'plan': 'oss',
			'https': false,
			'telnet': false,
			'unlocked': false,
			'unlocked_left': 0
		}


.. http:get:: /api/locations
	
	Get a list of the top 10,000 countries and cities that match the given search query.

	:query key: Shodan API key
	:query q: Search query

	.. note::
		Uses 1 query credit.

	**Example Request**:

	.. sourcecode:: http

		GET /api/locations?q=apache&key=SHODAN_API_KEY

	**Example Response**:

	.. sourcecode:: javascript

		{
			'total': 1124532
			'countries': [
				{
					'name': 'Switzerland',
					'code': 'CH',
					'count': 2352
				}
			],
			'cities': [
				{
					'name': 'Zurich',
					'count': 1204
				}
			]
		}


.. http:get:: /api/search

	Search Shodan for devices using a search query.

	:query key: Shodan API key
	:query q: Search query
	:query p: Page number

	.. note::

		Uses 1 query credit if:
			- Page number > 1
			- Search query contains any of the following filters
				+ city
				+ country
				+ net
				+ geo
				+ before
				+ after
				+ org
				+ isp
				+ title
				+ html

	**Example Request**:

	.. sourcecode:: http

		GET /api/search?q=apache&p=1&key=SHODAN_API_KEY

	**Example Response**:

	.. sourcecode:: javascript

		{
			'total': 10141204,
			'matches': [
				{
					'ip': '1.2.3.4',
					'port': 22,
					'updated': '24.01.2013',
					'data': 'OpenSSH 1.99'
				}
			]
		}
