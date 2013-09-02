
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

Using the Exploit DB
--------------------

The `Exploit DB <http://www.exploit-db.com>`_ website provides an archive of known exploits for
various applications. Using the Shodan API it's very easy now to access their archive from
within your program. Currently, there are 2 relevant functions: :rb:meth:`Shodan::WebAPI#exploitdb.search`
and :rb:meth:`Shodan::WebAPI#exploitdb.download`.

.. code-block:: ruby
	
	# Search Exploit DB
	results = api.exploitdb.search('PHP')
	
	# Print result information
	puts "Results found: #{results['total']}"
	
	results['matches'].each { |exploit|
		puts "#{exploit['id']}: #{exploit['description']}"
	}

The Exploit DB :rb:meth:`Shodan::WebAPI#exploitdb.search` function returns a dictionary very similar to
Shodan's :rb:meth:`Shodan::WebAPI#search` method. Below is an example of what it looks like:

.. code-block:: ruby

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

To download the actual exploit, we provide the exploit ID to the :rb:meth:`Shodan::WebAPI#exploitdb.download`
function:

.. code-block:: ruby
	
	# Download the file
	file = api.exploitdb.download(exploit['id'])
	
	# Show the contents
	puts "Filename: #{file['filename']}"
	puts "Content-type: #{file['content-type']}"
	puts file['data']

The resulting dictionary has 3 fields: `content-type`, `filename` and `data`.

Searching Metasploit Modules
----------------------------

The `Metasploit framework <http://www.metasploit.com>`_ is a highly popular and versatile platform
for penetration testing and security development. It offers more than a thousand modules that
contain a variety of functions. Using the Shodan API it's now possible to search the archive
of Metasploit modules, get information about them and even download their source code.

The 2 new functions are: :rb:meth:`Shodan::WebAPI#msf.search` and :rb:meth:`Shodan::WebAPI#msf.download`.
Lets take a look at the :rb:meth:`Shodan::WebAPI#msf.search` first:

.. code-block:: ruby
	
	# Search for a Metasploit module
	modules = api.msf.search('microsoft')
	
	# Print basic information about the module
	puts "Modules found: #{modules['total']}"
	results['matches'].each { |module|
		puts "#{module['type']}: #{module['name']}"

The information contained in the `module` hash is identical to what you find in the Metasploit documentation.
It contains everything from the module description, type, rank, references and more.

Once you've found the module you were looking for, use the :rb:meth:`Shodan::WebAPI#msf.download` function to
download the actual module code and see what it looks like:

.. code-block:: ruby
	
	# Download the file
	file = api.msf.download(module['fullname'])
	
	# Show the contents
	puts "Filename: #{file['filename']}"
	puts "Content-type: #{file['content-type']}"
	puts file['data']

The only argument you need to pass the function is the `fullname` of the module. I hope this has given you some
ideas on integrating Metasploit module information in your program.
