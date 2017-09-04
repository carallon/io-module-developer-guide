net.SslServer
#############

**Implements:** :doc:`tcpserver`

Used to create an SSL encrypted TCP server.

Methods
*******

SslServer.new() -> SslServer
============================

Creates a new SslServer.

SslServer:next_pending_secure_connection() -> SslSocket
=======================================================

Returns the next pending connection as a connected SslSocket.

The socket will be deleted when the server is garbage collected, so make sure you don't attempt to use it after discarding the server.

SslServer:next_pending_secure_connection_async()
================================================

Asynchronous version of ``SslServer:next_pending_secure_connection()``, which must be used when outside the server's event handlers.

The callback has the following signature:

.. code-block:: lua
   
   function(ssl_server, ssl_socket)

If there are pending connections, the next secure connection will be passed in the ``ssl_socket`` argument. If there are no pending connections, ``ssl_socket`` will be nil.
