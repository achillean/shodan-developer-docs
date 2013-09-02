
:mod:`shodan` -- Shodan Reference Documentation
===============================================

.. py:class:: WebAPI
	
	An object to interact with Shodan through the web services API.
	
	.. py:method:: __init__(api_key)
		
		Instantiate the object using an API key.
		
		:param api_key: Shodan API key
		:type api_key: string
	
	.. py:method:: host(ip)
		
		Get all available information on an IP.
		
		:param ip: IP of the computer
		:type ip: string
	
	.. py:method:: search(query)
		
		Search the Shodan database.
		
		:param query: search query; identical syntax to the website
		:type query: string
	
	.. py:method:: exploits.search(query, sources=[], cve=None, osvdb=None, msb=None, bid=None)
		
		Search the Shodan Exploits archive, which currently includes: Metasploit, Exploit DB, Packetstorm,
		CVE and OSVDB.
		
		:param query: search query; identical syntax to the website
		:type query: string
		:param sources: list of specific sources to search in, possible values: metasploit, exploitdb, cve, osvdb, packetstorm.
		:param cve: CVE identifier
		:param osvdb: OSVDB identifier
		:param msb: Microsoft Security Bulletin ID
		:param bid: Bugtraq ID
	
	.. py:method:: exploitdb.download(eid)
		
		Download the exploit code for the given ExploitDB ID.
		
		:param eid: ExploitDB identifier
		:type eid: int
	
	.. py:method:: exploitdb.search(query[, author=None, platform=None, port=None, type=None])
		
		Search the ExploitDB archive.
		
		:param query: search terms to look for in the exploit description
		:type query: string
		:param author: name of author
		:param code: text to search for in the exploit code itself
		:param cve: CVE ID for 
		:param platform: target platform (e.g. windows, linux, hardware, etc.)
		:param port: service port number
		:param type: One of the following: any, dos, local, papers, remote, shellcode, webapps.
	
	.. py:method:: msf.download(fullname)
		
		Download the module code for the given Metasploit module.
		
		:param fullname: fullname (path) of the Metasploit module
		:type fullname: str
	
	.. py:method:: msf.search(query, **kwargs)
		
		Search the Metasploit module archive.
		
		:param query: search terms to look for in the module description
		:type query: string
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
		
:mod:`shodan.wps` -- Wifi Positioning System
============================================

.. py:class:: GoogleLocation
	
	Locate the physical address of a MAC/ BSSID using Google Locations.
	
	.. py:method:: __init__()
		
		Instantiate the object.
	
	.. py:method:: locate(mac)
		
		Get the physical location of the given MAC address.
		
		:param mac: BSSID or MAC address of the device (ex. 00:1D:7E:F0:A2:B0)
		:type mac: string


	
.. 	.. py:method:: dataloss.search(**kwargs)
.. 		
.. 		Search the Dataloss DB archive.
.. 		
.. 		:param arrest: whether the incident resulted in an arrest
.. 		:type arrest: bool
.. 		:param breaches: the type of breach that occurred (Hack, MissingLaptop etc.)
.. 		:type breaches: string
.. 		:param country: country where the incident took place
.. 		:type country: string
.. 		:param ext: whether an external, third party was affected
.. 		:type ext: bool
.. 		:param ext_names: the name of the third party company that was affected
.. 		:type ext_names: string
.. 		:param lawsuit: whether the incident resulted in a lawsuit
.. 		:type lawsuit: bool
.. 		:param name: name of affected the company/ organization
.. 		:type name: string
.. 		:param records: the number of records that were lost/ stolen
.. 		:type records: int
.. 		:param recovered: whether the affected items were recovered
.. 		:type recovered: bool
.. 		:param sub_types: the sub-categorization of the affected company/ organization
.. 		:type sub_types: string
.. 		:param source: whether the incident occurred from inside or outside the organization
.. 		:type source: string
.. 		:param stocks: stock symbol of the affected company
.. 		:type stocks: string
.. 		:param types: the basic type of organization (government, business, educational)
.. 		:type types: string
.. 		:param uid: unique ID for the incident
.. 		:type uid: int