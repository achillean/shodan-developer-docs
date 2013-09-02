
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
	
	.. rb:method:: exploitdb.download(eid)
		
		Download the exploit code for the given Exploit DB ID.
		
		:param eid: Exploit DB identifier
		:type eid: Integer
	
	.. rb:method:: exploitdb.search(query[, params={}])
		
		Search the Exploit DB archive.
		
		Filters can be passed as named pairs to the function:
		
		.. code-block:: ruby
			
			exploitdb.search('PHP', :platform => 'php', :author => 'nuffsaid')
		
		:param query: search terms to look for in the exploit description
		:type query: String
		:param params: hash containing the search filters; possible keys are: author, platform, port, type, cve and code.

	.. rb:method:: msf.download(fullname)
		
		Download the module code for the given Metasploit module.
		
		:param fullname: fullname (path) of the Metasploit module
		:type fullname: String
	
	.. rb:method:: msf.search(query[, params={}])
		
		Search the Metasploit module archive.
		
		.. code-block:: ruby
			
			msf.search('', cve='2009-4324')
		
		:param query: search terms to look for in the module description
		:type query: String
		:param arch: architecture
		:param author: name of author
		:param bid: Bugtraq ID
		:param cve: CVE ID
		:param fullname: the fullname (path) of the module
		:param msb: Microsoft Security Bulletin ID
		:param name: module name
		:param osvdb: OSVDB identifier
		:param platform: target platform (e.g. windows, linux, etc.)
		:param privileged: True/ False
		:param rank: module rank
		:param type: exploit, payload, auxiliary etc.
		:param version: version of the module
		