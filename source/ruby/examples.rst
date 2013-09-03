
Examples
========

Generate IP list
----------------

.. code-block:: ruby
	
	#!/usr/bin/env ruby
	#
	# shodan_ips.py
	# Search SHODAN and print a list of IPs matching the query
	#
	# Author: achillean
	
	require 'rubygems'
	require 'shodan'
	
	# Configuration
	API_KEY = "YOUR_API_KEY"
	
	# Input validation
	if ARGV.length == 0
		puts "Usage: ./shodan_ips.rb <search query>"
		exit 1
	end
	
	begin
		# Setup the API
		api = Shodan::WebAPI.new(API_KEY)
	
		# Perform the search
		query = ARGV.join(" ")
		result = api.search(query)
		
		# Loop through the matches and print the IPs
		result['matches'].each{ |host|
			puts host['ip']
		}
	rescue Exception => e
		puts "Error: #{e.to_s}"
		exit 1
	end
