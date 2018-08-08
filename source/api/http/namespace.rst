HTTP namespace
##############

The ``http`` namespace has methods to allow an IO module to perform HTTP requests. The ``request()`` method allows any HTTP request to be created; there are some convenience methods for GET, PUT, POST and DELETE requests. All the request methods return an :doc:`./response` object.

Methods
*******

.. _http-request-method:

http.request(requestoptions, data, callback) -> Response
========================================================

Generic request function, for which the HTTP method must be specified in ``requestoptions``.

* ``requestoptions``: table with keys according to :ref:`request-options`.
* ``data``: :doc:`../payload` for the request.

The callback has the following signature:

.. code-block:: lua

   function(response)

http.get(requestoptions, callback) -> Response
==============================================

Sends an HTTP GET request to the endpoint specified in ``requestoptions``.

* ``requestoptions``: table with keys according to :ref:`request-options`.

The callback has the following signature:

.. code-block:: lua

   function(response)

http.put(requestoptions, data, callback) -> Response
====================================================

Sends an HTTP PUT request with the given data to the endpoint specified in ``requestoptions``.

* ``requestoptions``: table with keys according to :ref:`request-options`.
* ``data``: :doc:`../payload` for the request.

The callback has the following signature:

.. code-block:: lua

   function(response)


http.post(requestoptions, data, callback) -> Response
=====================================================

Sends an HTTP POST request with the given data to the endpoint specified in ``requestoptions``.

* ``requestoptions``: table with keys according to :ref:`request-options`.
* ``data``: :doc:`../payload` for the request.

The callback has the following signature:

.. code-block:: lua

   function(response)


http.delete(requestoptions, callback) -> Response
=================================================

Sends an HTTP DELETE request to the endpoint specified in ``requestoptions``.

* ``requestoptions``: table with keys according to :ref:`request-options`.

The callback has the following signature:

.. code-block:: lua

   function(response)


.. _request-options:

Request Options
***************

Request options are passed as a table to :ref:`http.request()<http-request-method>` and the HTTP request convenience methods. The recognised table keys and the supported values are as follows:

.. list-table::
   :widths: 1 2 6
   :header-rows: 1
   
   * - Key
     - Type
     - Description
   * - ``method``
     - string
     - The HTTP method/verb of the request, e.g. ``"PUT"``
   * - ``protocol``
     - string
     - The protocol part of the URL, e.g. ``"http:"`` or ``"https:"``
   * - ``hostname``
     - string
     - The host part of the URL, e.g. ``"192.168.0.1"``
   * - ``path``
     - string
     - The path part of the URL, e.g. ``"/json/api/path"``
   * - ``query``
     - string
     - The query string to be added to the URL, e.g. ``"page=5&order=ascending"``
   * - ``port``
     - number
     - The port of the URL; defaults to 80
   * - ``headers``
     - :ref:`http-headers`
     - Headers to be set for the HTTP request
   * - ``ssl_configuration``
     - :doc:`../net/sslconfiguration`
     - Optional. If given, overrides the default SSL configuration for communicating with servers requiring secure connections.

.. _http-headers:

HTTP Headers
************

HTTP requests and responses have headers, according to the HTTP specification. Headers must be set on requests and read from server responses. In the IO modules API, headers are mapped to/from a table. The keys of the table are the header names; the values are strings. For example:

.. code-block:: lua

   local headers = {
      ["Content-Type"] = 'application/json',
      ["Content-Length"] = '32',
      ["Authorization"] = 'Bearer IssuedBearerToken'
   }
