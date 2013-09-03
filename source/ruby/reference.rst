
:mod:`shodan` -- Shodan Reference Documentation
===============================================

.. rb:module:: Shodan

.. rb:class:: WebAPI
	
	An object to interact with Shodan through the web services API.
	
	.. rb:classmethod:: new(api_key)
		
		Instantiate the object using an API key.
		
		:param api_key: Shodan API key
		:type api_key: String
	
	.. rb:method:: host(ip)
		
		Get all available information on an IP.
		
		:param ip: IP of the computer
		:type ip: Integer
	
	.. rb:method:: search(query)
		
		Search the Shodan database.
		
		:param query: search query; identical syntax to the website
		:type query: String