net.UdpSocket
#############

UDP (User Datagram Protocol) is a lightweight, unreliable, datagram-oriented, connectionless protocol. It can be used when reliability isn't important. UdpSocket allows you to send and receive UDP datagrams.

You can bind to a local port using ``bind()``, then call the read/write datagram functions to transfer data. If you just want to send datagrams, you don't need to call ``bind()``.

UdpSocket calls the ``bytes_written_handler`` every time a datagram is written to the network.

The ``ready_read_handler`` is called whenever datagrams arrive. In that case, ``has_pending_datagrams`` will be true and ``pending_datagram_size`` will be the size of the first pending datagram. Call one of the read datagram functions to read it.

**Note:** An incoming datagram should be read when the ``ready_read_handler`` is called, otherwise this handler will not be called for the next datagram.

Properties
**********

UdpSocket.error
===============

Read only. The type of error that last occurred. Errors are static properties of UdpSocket, e.g. ``UdpSocket.NETWORK_ERROR``.

.. list-table::
   :widths: 2 1 5
   :header-rows: 1
   
   * - Error
     - Value
     - Description
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
   * - ``UNKNOWN_ERROR``
     - -1
     - An unidentified error occurred.

UdpSocket.error_string -> string
================================

Read only. A human-readable description of the last error that occurred.

UdpSocket.state
===============

Read only. The state of the socket. States are static properties of UdpSocket, e.g. ``UdpSocket.BOUND_STATE``.

.. list-table::
   :widths: 2 1 5
   :header-rows: 1
   
   * - State
     - Value
     - Description
   * - ``UNBOUND_STATE``
     - 0
     - The socket is not bound to a port.
   * - ``BOUND_STATE``
     - 4
     - The socket is bound to a port.

UdpSocket.local_address
=======================

Read only. The string representation of the host address of the local socket, if available, otherwise an empty string.

UdpSocket.local_port
====================

Read only. The port of the local socket if available, otherwise 0.

Methods
*******

UdpSocket.new() -> UdpSocket
============================

Creates a new UdpSocket.

UdpSocket:bind(port) -> bool
============================

Binds the socket exclusively to ``port``. If ``port`` is 0 then a random port is chosen.

After binding, the ``ready_read_handler`` is called whenever a UDP datagram is received by the controller on the specified port. Thus, this function is useful to write UDP servers.

On success, the functions returns true and the socket enters ``BOUND_STATE``; otherwise it returns false.

UdpSocket:bind_async(port, callback)
====================================

Asynchronous version of ``UdpSocket:bind()``, which must be used when outside the socket's event handlers.

The callback has the following signature:

.. code-block:: lua

   function(socket, result, error)

On success, ``result`` will be true. If an internal error occurs then ``error`` will be non-nil.

UdpSocket:close()
=================

Closes the socket. On closing, the socket enters ``UNBOUND_STATE``.

UdpSocket:read_datagram_bytes() -> table
========================================

Reads the datagram returns a table with the following entries:

* ``data`` - the data received, represented as a numerically-indexed table of bytes (stored as numbers).
* ``sender_address`` - the sender's address as a string, e.g. "43.195.83.32".
* ``sender_port`` - the sender's port number.
* ``valid`` - false if the operation failed, true otherwise.

If the operation fails data, ``sender_address`` and ``sender_port`` will be nil.

UdpSocket:read_datagram_bytes_async(callback)
=============================================

Asynchronous version of ``UdpSocket:read_datagram_bytes()``, which must be used when outside the socket's event handlers.

The callback has the following signature:

.. code-block:: lua

   function(socket, result)

``result`` will be as for the return value of ``UdpSocket:read_datagram_bytes()``.

UdpSocket:read_datagram_string() -> table
=========================================

Reads the datagram as a string and returns a table with the following entries:

* ``data`` - the data received, represented as a string.
* ``sender_address`` - the sender's address as a string, e.g. "43.195.83.32".
* ``sender_port`` - the sender's port number.
* ``valid`` - false if the operation failed, true otherwise.

If the operation fails data, ``sender_address`` and ``sender_port`` will be nil.

UdpSocket:read_datagram_string_async(callback)
==============================================

Asynchronous version of ``UdpSocket:read_datagram_string()``, which must be used when outside the socket's event handlers.

The callback has the following signature:

.. code-block:: lua

   function(socket, result)

``result`` will be as for the return value of ``UdpSocket:read_datagram_string()``.

UdpSocket:write_datagram_bytes(table, hostname, port) -> number
===============================================================

Sends a datagram to ``hostname`` at the given ``port``. If ``table`` is a numerically-indexed table of bytes, these bytes will be used as the data payload. Returns the number of bytes written, or -1 if an error occurs.

The ``hostname`` may be an IP address in string form (e.g. "43.195.83.32"), or it may be a host name (e.g. "www.example.com").

UdpSocket:write_datagram_bytes_async(table, hostname, port, callback)
=====================================================================

Asynchronous version of ``UdpSocket:write_datagram_bytes()``, which must be used when outside the socket's event handlers.

The callback has the following signature:

.. code-block:: lua

   function(socket, result, error)

``result`` will be as for the return value of ``UdpSocket:write_datagram_bytes()``. If an internal error occurs then ``error`` will be non-nil.

UdpSocket:write_datagram_string(string, hostname, port) -> number
=================================================================

Sends a datagram to ``hostname`` at the given ``port``. ``string`` is used as the data payload. Returns the number of bytes written, or -1 if an error occurs.

The ``hostname`` may be an IP address in string form (e.g. "43.195.83.32"), or it may be a host name (e.g. "www.example.com").

UdpSocket:write_datagram_string_async(string, hostname, port, callback)
=======================================================================

Asynchronous version of ``UdpSocket:write_datagram_string()``, which must be used when outside the socket's event handlers.

The callback has the following signature:

.. code-block:: lua

   function(socket, result, error)

``result`` will be as for the return value of ``UdpSocket:write_datagram_string()``. If an internal error occurs then ``error`` will be non-nil.

UdpSocket:join_multicast_group(group_address) -> bool
=====================================================

Join the multicast group specified by ``group_address``. ``group_address`` must be a valid IP address in string form; the operation will only succeed if ``group_address`` is a valid multicast address, e.g. "224.0.0.0".

The operation will fail if the socket is not in ``BOUND_STATE``.

Returns true if successful; otherwise the ``error`` property will be set accordingly.

UdpSocket:join_multicast_group_async(group_address, callback)
=============================================================

Asynchronous version of ``UdpSocket:join_multicast_group()``, which must be used when outside the socket's event handlers.

The callback has the following signature:

.. code-block:: lua

   function(socket, result, error)

``result`` will be as for the return value of ``UdpSocket:join_multicast_group()``. If an internal error occurs then ``error`` will be non-nil.

UdpSocket:leave_multicast_group(group_address) -> bool
======================================================

Leave the multicast group specified by ``group_address``. ``group_address`` must be a valid IP address in string form.

The operation will fail if the socket is not in ``BOUND_STATE``.

Returns true if successful; otherwise the ``error`` property will be set accordingly.

UdpSocket:leave_multicast_group_async(group_address, callback)
==============================================================

Asynchronous version of ``UdpSocket:leave_multicast_group()``, which must be used when outside the socket's event handlers.

The callback has the following signature:

.. code-block:: lua

   function(socket, result, error)

``result`` will be as for the return value of ``UdpSocket:leave_multicast_group()``. If an internal error occurs then ``error`` will be non-nil.

Event handlers
**************

UdpSocket.bytes_written_handler
===============================

The handler has the following signature:

.. code-block:: lua

   function(socket, bytes)

The handler is called every time a datagram is written to the network.

UdpSocket.error_handler
=======================

The handler has the following signature:

.. code-block:: lua

   function(socket, error)

The handler is called when an error occurs. The error types are given with the ``error`` property. The ``error_string`` property will give a human-readable description of the error.

UdpSocket.ready_read_handler
============================

The handler has the following signature:

.. code-block:: lua

   function(socket)

The handler is called when datagrams arrive, but only once after each call to a read datagram function.

UdpSocket.state_changed_handler
===============================

The handler has the following signature:

.. code-block:: lua

   function(socket, state)

The handler is called when the socket state changes. The state types are given with the ``state`` property.
