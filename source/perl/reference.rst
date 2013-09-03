
:mod:`Shodan` -- Shodan Reference Documentation
===============================================

.. cpp:namespace:: Shodan

.. cpp:class:: WebAPI
	
	An object to interact with Shodan through the web services API.
	
	.. cpp:function:: new(api_key)
		
		Instantiate the object using an API key.
		
		:parameters:
			
			**api_key**: Shodan API key (`string`)
	
	.. cpp:function:: host(ip)
		
		Get all available information on an IP.
		
		:parameters:
			
			**ip**: IP of the computer (`int`)
	
	.. cpp:function:: search(query)
		
		Search the Shodan database.
		
		:parameters:
			
			**query**: search query; identical syntax to the website (`string`)