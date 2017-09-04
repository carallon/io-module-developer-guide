net.TcpServer
#############

Used to create a TCP server --- this object makes it possible to accept incoming TCP connections. You can specify the port or TcpServer will pick one automatically.

Call ``listen()`` to tell the server to listen for incoming connections. The ``new_connect_handler`` is then called each time a client connects to the server.

Call ``next_pending_connection()`` to accept the pending connection as a connected TcpSocket. The method returns a TcpSocket in ``CONNECTED_STATE`` that you can use for communicating with the client.

If an error occurs, the ``error_string`` property will have a human-readable description of what happened.

When listening for connections, the port on which the server is listening is available from the ``port`` property.

Calling ``close()`` makes TcpServer stop listening for incoming connections.

Properties
**********

TcpServer.listening -> bool
===========================

Read only. True if the server is listening for connections.

TcpServer.port -> number
========================

Read only. The port on which the server is listening, or 0 if the server is not listening for connections.

TcpServer.max_pending_connections -> number
===========================================

Read/write. The maximum number of pending connections that the server will accept. The default limit is 30 connections.

TcpServer.has_pending_connections -> bool
=========================================

Read only. True if the server has any pending connections.

TcpServer.error_string -> string
================================

Read only. A human-readable description of the last error that occurred.

Methods
*******

TcpServer.new() -> TcpServer
============================

Creates a new TcpServer.

TcpServer:listen(port) -> bool
==============================

Tells the server to start listening for incoming connections on ``port``. If ``port`` is 0, a port is chosen automatically. Returns true on success.

TcpServer:listen_async(port, callback)
======================================

Asynchronous version of ``TcpServer:listen()``, which must be used outside of the server's event handlers.

The callback has the following signature:

.. code-block:: lua

   function(server, result, error)

On success, ``result`` will be true. If an internal error occurs then ``error`` will be non-nil.

TcpServer:pause_accepting()
===========================

Pauses accepting new connections. Queued connections will remain in the queue.

TcpServer:resume_accepting()
============================

Resumes accepting new connections.

TcpServer:next_pending_connection() -> TcpSocket
================================================

Returns the next pending connection as a connected TcpSocket.

The socket will be deleted when the server is garbage collected, so make sure you don't attempt to use it after discarding the server.

TcpServer:next_pending_connection_async()
=========================================

Asynchronous version of ``TcpServer:next_pending_connection()``, which must be used outside of the server's event handlers.

The callback has the following signature:

.. code-block:: lua

   function(server, socket)

If there are pending connections, the next connection will be passed in the ``socket`` argument. If there are no pending connections, ``socket`` will be nil.

TcpServer:close()
=================

Closes the server - it will no longer listen for incoming connections.

Event handlers
**************

TcpServer.accept_error_handler
==============================

The handler has the following signature:

.. code-block:: lua

   function(server, error)

The handler is called when a new connection results in an error. ``error`` is a socket error.

TcpServer.new_connection_handler
================================

The handler has the following signature:

.. code-block:: lua

   function(server)

The handler is called every time a new connection is available.
