
Scanhub REST API
================

.. http:get:: /api/scanhub/(slug)
	
	Get the information about a Scanhub based on its slug (a sanitized version of its name)

	:query key:			Shodan API key
	:query slug:		The sanitized name of the Scanhub as shown in a URL

	**Example Request**

	.. sourcecode:: http

		GET /api/scanhub/scanhub-demo&key=SHODAN_API_KEY

	**Example Response**

	.. sourcecode:: javascript

		HTTP/1.1 200 OK

		{
			'name': 'Scanhub Demo',
			'slug': 'scanhub-demo',
			'description': 'An example Scanhub',
			'public': true
		}


.. http:post:: /api/scanhub

	Creates a new Scanhub.

	:query key:			Shodan API key
	:query name:		Name of the new Scanhub
	:query description:	A description of the Scanhub so others know what its purpose is
	:query public:		True if the Scanhub should be public, False otherwise

	**Example Request**:

	.. sourcecode:: http

		POST /api/scanhub

		key=SHODAN_API_KEY&name=Scanhub Demo&description=An example Scanhub&public=True


	**Example Response**:

	.. sourcecode:: http

		HTTP/1.1 301 Moved Permanently
		Location: /api/scanhub/scanhub-demo
