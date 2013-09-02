
Tutorial
========

Installation
------------------

To get started with the Perl library for Shodan, first make sure that you've
`received your API key <http://www.shodanhq.com/api_doc>`_. Once that's done,
install the library:

.. code-block:: bash
	
	# Install the dependencies
	sudo perl -MCPAN -e 'install CGI::Enurl'
	sudo perl -MCPAN -e 'install JSON::XS'
	sudo perl -MCPAN -e 'install HTTP::Request::Common'
	
	# Install Shodan
	curl -OL http://github.com/downloads/achillean/shodan-perl/Shodan-0.3.tar.gz
	tar zxvf Shodan-0.3.tar.gz
	cd Shodan-0.3
	perl Makefile.PL
	make
	sudo make install


Getting started
---------------

The first thing we need to do in our code is to initialize the API object:

.. code-block:: perl

	use Shodan::WebAPI;
	
	$SHODAN_API_KEY = "insert your API key here";
	
	$api = new Shodan::WebAPI($SHODAN_API_KEY);

	
Searching Shodan
----------------

Now that we have our API object all good to go, we're ready to perform a search:

.. code-block:: perl
	
	# Search Shodan
	$results = $api->search("apache");
	
	# Check for errors
	if ( $result->{'error'} ) {
		print "Error: " . $result->{'error'} . "\n";
	}
	else {
		@matches = @{$results->{'matches'}};
		for ( $i = 0; $i < $#matches; $i++ ) {
			print "IP: $matches[$i]->{ip}\n";
			print "$matches[$i]->{data}\n\n";
		}
	}

Stepping through the code, we first call the :cpp:func:`WebAPI::search` method on the `api` object which
returns a hash of result information. We then print how many results were found in total,
and finally loop through the returned matches and print their IP and banner.

There's a lot more information that gets returned by the function. See below for a shortened example
dictionary that :cpp:func:`WebAPI::search` returns:

.. code-block:: perl
	
	{
		total => 8669969,
		countries => [
			{
				code => 'US',
				count => 4165703,
				name => 'United States'
			},
			{code => 'DE', count => 610270, name => 'Germany'},
			{code => 'JP', count => 496556, name => 'Japan'},
			{code => 'RO', count => 486107, name => 'Romania'},
			{code => 'GB', count => 273948, name => 'United Kingdom'}
		],
		matches => [
			{
				country => 'DE',
				data => 'HTTP/1.0 200 OK\r\nDate: Mon, 08 Nov 2010 05:09:59 GMT\r\nSer...',
				hostnames => ['pl4t1n.de'],
				ip => '89.110.147.239',
				os => 'FreeBSD 4.4',
				port => 80,
				updated => '08.11.2010'
			},
			...
		]
	}

It's also good practice to always check the returned hash for the 'error' key,
which contains an explanation for why the API call failed if something went wrong.
But for simplicity's sake, I will leave that part out from now on.

Looking up a host
-----------------

To see what Shodan has available on a specific IP we can use the :cpp:func:`WebAPI::host` function:

.. code-block:: perl
	
	# Lookup the host
	$host = $api->host('217.140.75.46')
	
	# Print general info
	print "IP: $host{ip}\n";
	print "Country: $host{country}\n";
	print "City: $host{city}\n";
	
	# Print all banners
	@banners = @{ $host->{'data'} };
	for ( $i = 0; $i < $#banners; $i++ ) {
		print "Port: $banners[$i]{port}\n";
		print "Banner: $banners[$i]{banner}\n\n";
	}

Using the Exploit DB
--------------------

The `Exploit DB <http://www.exploit-db.com>`_ website provides an archive of known exploits for
various applications. Using the Shodan API it's very easy now to access their archive from
within your program. Currently, there are 2 relevant functions: :cpp:func:`WebAPI::exploitdb_search`
and :cpp:func:`WebAPI::exploitdb_download`.

.. code-block:: perl
	
	# Search Exploit DB
	$results = $api->exploitdb_search('PHP')
	
	# Print result information
	print "Results found: $results->{total}\n";
	@matches = @{ $results->{matches} };
	for ( $i = 0; $i < $#matches; $i++ ) {
		print "$matches[$i]{id}: $matches[$i]{description}\n";
	}

The Exploit DB :cpp:func:`WebAPI::exploitdb_search` function returns a dictionary very similar to
Shodan's :cpp:func:`WebAPI::search` method. Below is an example of what it looks like:

.. code-block:: perl

	{
		total => 2192,
		matches => [
			{
				author => 'MP',
				date => '18.10.2006',
				description => 'Php AMX 0.90 (plugins/main.php) Remote File Include Vulnerability',
				id => 2591,
				platform => 'php',
				port => 0,
				type => 'webapps'
				cve => '2010-3709'
			},
			...
		]
	}

To download the actual exploit, we provide the exploit ID to the :cpp:func:`WebAPI::exploitdb_download`
function:

.. code-block:: perl
	
	# Download the file
	$file = $api->exploitdb.download(exploit{id});
	
	# Show the contents
	print "Filename: $file->{filename}\n";
	print "Content-type: $file->{'content-type'}\n";
	print file->{data};

The resulting dictionary has 3 fields: `content-type`, `filename` and `data`.

Searching Metasploit Modules
----------------------------

The `Metasploit framework <http://www.metasploit.com>`_ is a highly popular and versatile platform
for penetration testing and security development. It offers more than a thousand modules that
contain a variety of functions. Using the Shodan API it's now possible to search the archive
of Metasploit modules, get information about them and even download their source code.

The 2 new functions are: :cpp:func:`WebAPI::msf_search` and :cpp:func:`WebAPI::msf_download`.
Lets take a look at the :cpp:func:`WebAPI::msf_search` first:

.. code-block:: perl
	
	# Search for a Metasploit module
	$results = $api->msf_search('microsoft')
	
	# Print result information
	print "Results found: $results->{total}\n";
	@modules = @{ $results->{matches} };
	for ( $i = 0; $i < $#modules; $i++ ) {
		print "$modules[$i]{type}: $modules[$i]{name}\n";
	}

The information contained in the `module` hash is identical to what you find in the Metasploit documentation.
It contains everything from the module description, type, rank, references and more.

Once you've found the module you were looking for, use the :cpp:func:`WebAPI::msf_download` function to
download the actual module code and see what it looks like:

.. code-block:: perl
	
	# Download the file
	$file = $api->msf_download( module{ fullname })
	
	# Show the contents
	print "Filename: $file->{filename}\n";
	print "Content-type: $file->{'content-type'}\n";
	print file->{data};

The only argument you need to pass the function is the `fullname` of the module. I hope this has given you some
ideas on integrating Metasploit module information in your program.

