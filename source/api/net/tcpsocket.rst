net.TcpSocket
#############

**Implements:** :doc:`../stream`

TCP (Transmission Control Protocol) is a reliable, stream-oriented, connection-oriented transport protocol. UDP (User Datagram Protocol) is an unreliable, datagram-oriented, connectionless protocol. In practice, this means that TCP is better suited for continuous transmission of data, whereas the more lightweight UDP can be used when reliability isn't important.

At any time, TcpSocket has a state (accessed via the ``state`` property). The initial state is ``UNCONNECTED_STATE``. After calling ``connect()``, the socket first enters ``HOST_LOOKUP_STATE``. If the host is found, TcpSocket enters ``CONNECTING_STATE`` and the ``host_found`` event handler is called. When the connection has been established, it enters ``CONNECTED_STATE`` and the connected event handler is called. If an error occurs at any stage, the error event handler is called. Whenever the state changes, the ``state_changed`` event handler is called. For convenience, the valid property is true if the socket is ready for reading and writing, but note that the socket's state must be ``CONNECTED_STATE`` before reading and writing can occur.

Read or write data by calling ``read()`` or ``write()`` (or their async equivalents if performing these actions outside of an event handler), or use the convenience functions ``read_line()`` and ``read_all()``. The ``bytes_written`` event handler is called when data has been written to the socket.

The ``ready_read_handler`` is called every time a new chunk of data has arrived. The ``bytes_available`` property is then set to the number of bytes that are available for reading. Typically, you would read all available data in your ``ready_read_handler``. If you don't read all the data at once, the remaining data will still be available later, and any new incoming data will be appended to TcpSocket's internal read buffer.

To close the socket, call ``disconnect()``. TcpSocket enters ``CLOSING_STATE``. After all pending data has been written to the socket, TcpSocket actually closes the socket, enters ``UNCONNECTED_STATE``, and calls the ``disconnected_handler``. If you want to abort a connection immediately, discarding all pending data, call ``abort()`` instead. If the remote host closes the connection, TcpSocket will call the ``error_handler`` with ``REMOTE_HOST_CLOSED_ERROR``, during which the socket state will still be ``CONNECTED_STATE``, and then the ``disconnected_handler`` will be called.

The port and address of the remote device are available in ``remote_port`` and ``remote_address``. ``local_port`` and ``local_address`` are set to the port and address of the local socket.

Properties
**********

TcpSocket.error
===============

Read only. The type of error that last occurred. Errors are static properties of TcpSocket, e.g. ``TcpSocket.NETWORK_ERROR``.

.. list-table::
   :widths: 2 1 5
   :header-rows: 1
   
   * - Error
     - Value
     - Description
   * - ``CONNECTION_REFUSED_ERROR``
     - 0
     - The connection was refused by the peer (or timed out).
   * - ``REMOTE_HOST_CLOSED_ERROR``
     - 1
     - The remote host closed the connection. Note that the client socket (i.e. this socket) will be closed after the remote close notification has been sent.
   * - ``HOST_NOT_FOUND_ERROR``
     - 2
     - The host address was not found.
   * - ``SOCKET_RESOURCE_ERROR``
     - 4
     - The controller ran out of resources to handle this socket (e.g. there are too many sockets open).
   * - ``SOCKET_TIMEOUT_ERROR``
     - 5
     - The socket operation timed out.
   * - ``DATAGRAM_TOO_LARGE_ERROR``
     - 6
     - The datagram was larger than the controller's limit.
   * - ``NETWORK_ERROR``
     - 7
     - An error occurred with the network (e.g. the network cable was accidentally unplugged).
   * - ``UNSUPPORTED_SOCKET_OPERATION_ERROR``
     - 10
     - The requested socket operation is not supported by the controller.
   * - ``UNFINISHED_SOCKET_OPERATION_ERROR``
     - 11
     - The last socket operation has not finished yet.
   * - ``OPERATION_ERROR``
     - 19
     - An operation was attempted while the socket was in a state that did not permit it.
   * - ``TEMPORARY_ERROR``
     - 22
     - A temporary error occurred (e.g. operation would block and socket is non-blocking).
   * - ``UNKNOWN_ERROR``
     - -1
     - An unidentified error occurred.

TcpSocket.state
===============

Read only. The state of the socket. States are static properties of TcpSocket, e.g. ``TcpSocket.CONNECTED_STATE``.

.. list-table::
   :widths: 2 1 5
   :header-rows: 1
   
   * - State
     - Value
     - Description
   * - ``UNCONNECTED_STATE``
     - 0
     - The socket is not connected.
   * - ``HOST_LOOKUP_STATE``
     - 1
     - The socket is performing a host name lookup.
   * - ``CONNECTING_STATE``
     - 2
     - The socket has started establishing a connection.
   * - ``CONNECTED_STATE``
     - 3
     - A connection is established.
   * - ``BOUND_STATE``
     - 4
     - The socket is bound to a port.
   * - ``CLOSING_STATE``
     - 5
     - The socket is about to close (data may still be waiting to be written).

TcpSocket.valid
===============

Read only. True if the socket is valid and ready for use. Note: the socket must be in ``CONNECTED_STATE`` before reading and writing can occur.

TcpSocket.local_address
=======================

Read only. The string representation of the host address of the local socket, if available, otherwise an empty string.

TcpSocket.local_port
====================

Read only. The port of the local socket if available, otherwise 0.

TcpSocket.remote_address
========================

Read only. The address of the remote client if the socket is in ``CONNECTED_STATE``, or an empty string if ``connect()`` has not been called.

TcpSocket.remote_port
=====================

Read only. The port of the remote client if the socket is in ``CONNECTED_STATE``, otherwise 0.

Methods
**************

TcpSocket.new() -> TcpSocket
============================

Creates a new TcpSocket.

TcpSocket:connect(hostname, port, openmode)
===========================================

Attempts to make a connection to ``hostname`` on the given ``port``.

The socket is opened in the given ``open_mode`` and first enters ``HOST_LOOKUP_STATE``, then performs a host name lookup of ``hostname``. If the lookup succeeds, the ``host_found_handler`` is called and the socket enters ``CONNECTING_STATE``. It then attempts to connect to the address or addresses returned by the lookup. Finally, if a connection is established, the socket enters ``CONNECTED_STATE`` and the ``connected_handler`` is called.

At any point, the socket can call the ``error_handler`` if an error occurs.

The hostname may be an IP address in string form (e.g. "43.195.83.32"), or it may be a host name (e.g. "example.com"). TcpSocket will do a lookup only if required. ``port`` is in little endian byte order.

TcpSocket:disconnect()
======================

Attempts to close the socket. If there is pending data waiting to be written, the socket will enter ``CLOSING_STATE`` and wait until all data has been written. Eventually, it will enter ``UNCONNECTED_STATE`` and call the ``disconnected_handler``.

TcpSocket:abort()
=================

Aborts the current connection and resets the socket. Unlike ``disconnect()``, this function immediately closes the socket, discarding any pending data in the write buffer.

TcpSocket:bind(port) -> bool
============================

Specifies which port to use for an outgoing connection. If ``port`` is 0 then a random port is chosen.
On success, the functions returns true and the socket enters ``BOUND_STATE``; otherwise it returns false.

TcpSocket:bind_async(port, callback)
====================================

Asynchronous version of ``TcpSocket:bind()``, which must be used when outside the socket's event handlers.

The callback has the following signature:

.. code-block:: lua

   function(socket, result, error)

On success, ``result`` will be true. If an internal error occurs then ``error`` will be non-nil.

Event handlers
**************

TcpSocket.connected_handler
===========================

The handler has the following signature:

.. code-block:: lua

   function(socket)

The handler is called when a connection has been successfully established, after ``connect()`` has been called.

TcpSocket.disconnected_handler
==============================

The handler has the following signature:

.. code-block:: lua

   function(socket)

The handler is called when the socket has been disconnected.

TcpSocket.error_handler
=======================

The handler has the following signature:

.. code-block:: lua

   function(socket, error)

The handler is called when an error occurs. The error types are given with the ``error`` property. The ``error_string`` property will give a human-readable description of the error.

TcpSocket.state_changed_handler
===============================

The handler has the following signature:

.. code-block:: lua

   function(socket, state)

The handler is called when the socket state changes. The state types are given with the ``state`` property.

TcpSocket.host_found_handler
============================

The callback has the following signature:

.. code-block:: lua

   function(socket)

The handler is called after ``connect()`` has been called and the host lookup has succeeded.
