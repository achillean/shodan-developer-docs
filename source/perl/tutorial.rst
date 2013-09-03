
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