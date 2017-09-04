http.Response
#############

**Implements:** :doc:`../stream` (read only)

The Response object is returned from http requests or passed as an argument to request callbacks. It represents the incoming message from the HTTP server. It implements the readable stream interface.

Properties
**********

Response.status_code
====================

HTTP status code, e.g. 200, 500, etc.

Response.status_message
=======================

HTTP response status message, e.g. *OK* or *Internal Server Error*.

Response.error
==============

The error found during the processing of the request. Errors are static properties of Response, e.g. ``Response.READ_MODE``. If no error occurred, this will be set to ``NO_ERROR``.

.. list-table::
   :widths: 2 1 5
   :header-rows: 1

   * - Error
     - Value
     - Description
   * - ``NO_ERROR``
     - 0
     - No error occurred.
   * - ``CONNECTION_REFUSED_ERROR``
     - 1
     - The remote server refused the connection (the server is not accepting requests).
   * - ``REMOTE_HOST_CLOSED_ERROR``
     - 2
     - The remote server closed the connection prematurely, before the entire reply was received and processed.
   * - ``HOST_NOT_FOUND_ERROR``
     - 3
     - The remote host name was not found (invalid hostname).
   * - ``TIMEOUT_ERROR``
     - 4
     - The connection to the remote server timed out.
   * - ``OPERATION_CANCELLED_ERROR``
     - 5
     - The operation was cancelLed via calls to ``abort()`` or ``close()`` before it was finished.
   * - ``SSL_HANDSHAKE_FAILED_ERROR``
     - 6
     - The SSL/TLS handshake failed and the encrypted channel could not be established.
   * - ``TEMPORARY_NETWORK_FAILURE_ERROR``
     - 7
     - The connection was broken due to disconnection from the network, however the request should be resubmitted and will be processed as soon as the connection is re-established.
   * - ``NETWORK_SESSION_FAILED_ERROR``
     - 8
     - The connection was broken due to disconnection from the network or failure to start the network.
   * - ``TOO_MANY_REDIRECTS_ERROR``
     - 10
     - While following redirects, the maximum limit of 50 was reached.
   * - ``INSECURE_REDIRECT_ERROR``
     - 11
     - While following redirects, a redirect from an encrypted protocol (HTTPS) to an unencrypted one (HTTP) was detected.
   * - ``UNKNOWN_NETWORK_ERROR``
     - 99
     - An unknown network-related error was detected.
   * - ``CONTENT_ACCESS_DENIED``
     - 201
     - The access to the remote content was denied.
   * - ``CONTENT_OPERATION_NOT_PERMITTED_ERROR``
     - 202
     - The operation on the remote content is not permitted.
   * - ``CONTENT_NOT_FOUND_ERROR``
     - 203
     - The remote content was not found at the server.
   * - ``AUTHENTICATION_REQUIRED_ERROR``
     - 204
     - The remote server requires authentication to serve the content but the credentials provided were not accepted.
   * - ``CONTENT_RE_SEND_ERROR``
     - 205
     - The request needed to be sent again but this failed, for example because the upload data could not be read a second time.
   * - ``CONTENT_CONFLICT_ERROR``
     - 206
     - The request could not be completed due to a conflict with the current state of the resource.
   * - ``CONTENT_GONE_ERROR``
     - 207
     - The requested resource is no longer available at the server.
   * - ``UNKNOWN_CONTENT_ERROR``
     - 299
     - An unknown error related to the remote content was detected.
   * - ``PROTOCOL_UNKNOWN_ERROR``
     - 301
     - The controller cannot process the request because the protocol is not known.
   * - ``PROTOCOL_INVALID_OPERATION_ERROR``
     - 302
     - The requested operation is invalid for this protocol.
   * - ``PROTOCOL_FAILURE``
     - 399
     - A breakdown in protocol was detected (parsing error, invalid or unexpected responses, etc.)
   * - ``INTERNAL_SERVER_ERROR``
     - 401
     - The server encountered an unexpected condition which prevented it from fulfilling the request.
   * - ``OPERATION_NOT_IMPLEMENTED_ERROR``
     - 402
     - The server does not support the functionality required to fulfill the request.
   * - ``SERVICE_UNAVAILABLE_ERROR``
     - 403
     - The server is unable to handle the request at this time.
   * - ``UNKNOWN_SERVER_ERROR``
     - 499
     - An unknown error related to the server response was detected.

Response.headers
================

Table of headers in the response (see :ref:`http-headers`).

Event handlers
**************

Response.download_progress_handler
==================================

The handler has the following signature:

.. code-block:: lua

   function(response, bytesreceived, bytestotal)

The handler is called to indicate download progress of the request, if any, otherwise it's called once with ``bytesreceived`` and ``bytestotal`` both at 0. If the download size is not known, ``bytestotal`` will be -1.

Response.upload_progresss_handler
=================================

The handler has the following signature:

.. code-block:: lua

   function(response, bytessent, bytestotal)

The handler is called to indicate upload progress of the request, if any, otherwise it's not called.

Response.finished_handler
=========================

The handler has the following signature:

.. code-block:: lua

   function(response)

The handler is called when all the data has been received from the HTTP server.

Response.error_handler
======================

The handler has the following signature:

.. code-block:: lua

   function(response, error)

The handler is called if an error occurs while processing the response. The ``finished_handler`` will probably be called next.
