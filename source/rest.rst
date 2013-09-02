
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

	:query key: Shodan API key
	:query q: Search query

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

		If the the `page number` parameter is greater than 1 the API request consumes a credit.

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


.. http:get:: /api/search_exploits
	
	Search the Shodan Exploits database for vulnerability information.

	:query key: Shodan API key

	**Example Request**:

	.. sourcecode:: http

		GET /api/search_exploits

	**Example Response**:

	.. sourcecode:: javascript

		{
			'total': 1402,
			'sources': [
				['osvdb', 665],
				['cve', 539],
				['exploitdb', 102],
				['packetstorm', 71],
				['metasploit', 25]
			],
			'matches': [
				{
					'name': 'Multiple XSS in Apache OFBiz',
					'source_link': 'www.exploit-db.com',
					'source': 'Exploit DB',
					'link': 'http://www.exploit-db.com/exploits/12330',
					'references': [
						['CVE', '2010-0432']
					],
					'id': 12330,
					'desc': null
				}
			]
		}


.. http:get:: /api/exploitdb/search
	
	Search the ExploitDB database.

	:query key: Shodan API key
	:query q: Search query
	:query author: Author of the exploit
	:query platform: Platform that the exploit targets
	:query type: Type of the exploit (ex. webapps)
	:query port: Port that the service runs on for remote exploits
	:query cve: CVE identifier
	:query code: Search the content of the exploit itself
	:query id: ExploitDB identifier

	**Example Request**:

	.. sourcecode:: http

		GET /api/exploitdb/search?q=apache&key=SHODAN_API_KEY

	**Example Response**:

	.. sourcecode:: javascript

		{
			'matches': [
				{
					'author': 'Lucas Apa',
					'cve': '2010-0432',
					'date': '21.03.2010',
					'description': 'Multiple XSS in Apache OFBiz',
					'id': 12330,
					'platform': 'php',
					'port': 0,
					'type': 'webapps'
				}
			]
		}


.. http:get:: /api/exploitdb/download

	Download the exploit code for an ExploitDB entry.

	:query key: Shodan API key
	:query id: ExploitDB identifier

	**Example Request**:

	.. sourcecode:: http

		GET /api/exploitdb/download?id=12330&key=SHODAN_API_KEY


.. http:get:: /api/msf/search

	:query key: Shodan API key


.. http:get:: /api/msf/download

	:query key: Shodan API key

