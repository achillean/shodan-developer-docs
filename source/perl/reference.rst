
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
	
	.. cpp:function:: exploitdb_download(eid)
		
		Download the exploit code for the given Exploit DB ID.
		
		:parameters:
			
			**eid**: Exploit DB identifier (`int`)
	
	.. cpp:function:: exploitdb_search(query, params)
		
		Search the Exploit DB archive.
		
		Filters can be passed as named pairs to the function:
		
		.. code-block:: perl
			
			exploitdb_search('PHP', {platform => 'php', author => 'nuffsaid'})
		
		:parameters:
			
			**query**: search terms to look for in the exploit description (`string`)
			
			**params**: hash containing the search filters; possible keys are: author, platform, port, type, cve and code.

	.. cpp:function:: msf_download(fullname)
		
		Download the module code for the given Metasploit module.
		
		:parameters:
			
			**fullname**: fullname (path) of the Metasploit module (`string`)
	
	.. cpp:function:: msf_search(query, params)
		
		Search the Metasploit module archive.
		
		.. code-block:: perl
			
			msf_search('', {cve => '2009-4324'})
		
		:parameters:
			
			**query**: search terms to look for in the module description (`string`)
			
			**params**: hash containing the search filters; possible keys are: arch, author, bid, cve, fullname, msb, name, osvdb, platform, privileged, rank, type and version.
