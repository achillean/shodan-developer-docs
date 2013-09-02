
Examples
========

Generate IP list
----------------

.. code-block:: perl
	
	#!/usr/bin/env perl
	#
	# shodan_ips.pl
	# Search SHODAN and print a list of IPs matching the query
	#
	# Author: achillean
	
	use Shodan::WebAPI;
	
	use strict;
	use warnings;
	
	# Configuration
	my $API_KEY = "YOUR_API_KEY";
	
	# Must give at least 1 argument
	if ($#ARGV == -1) {
		print "Usage: ./shodan_ips.pl <search query>\n";
		exit 1;
	}
	
	# Setup the API
	my $api = new Shodan::WebAPI( $API_KEY );
	
	# Perform the search (returns a hash)
	my $query = join(" ", @ARGV);
	my $result = $api->search($query);
	
	# Check for an error
	if ( $result->{'error'} ) {
		print "Error: " . $result->{'error'} . "\n";
		exit 1;
	}
	
	# Loop through the matches and print the IP for each host
	my @matches = @{$result->{'matches'}};
	for ( my $i = 0; $i < $#matches; $i++ ) {
		print "$matches[$i]{ip}\n";
	}


The script can also be downloaded from http://www.surtri.com/shodan_ips.pl.
