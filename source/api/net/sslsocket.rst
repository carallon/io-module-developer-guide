net.SslSocket
#############

**Implements:** :doc:`tcpsocket`

The SslSocket object provides an SSL encrypted socket for both clients and servers.

SslSocket establishes a secure, encrypted TCP connection you can use for transmitting encrypted data. It supports modern SSL protocols, including SSL 3 and TLS 1.2.

SSL encryption operates on top of the existing TCP stream after the socket enters ``CONNECTED_STATE``. There are two simple ways to establish a secure connection using SslSocket: with an immediate SSL handshake, or with a delayed SSL handshake occurring after the connection has been established in unencrypted mode.

The most common way to use SslSocket is to construct an object and start a secure connection by calling ``connect_encrypted()``. This method starts an immediate SSL handshake once the connection has been established.

.. code-block:: lua

   socket = iomodules.net.SslSocket.new()
   socket.encrypted_handler = function()
       -- start talking now that encryption has begun
   end
   socket:connect_encrypted("chat.example.com", 993, net.Stream.READ_WRITE_MODE)

As with a plain TcpSocket, SslSocket enters the ``HOST_LOOKUP_STATE``, ``CONNECTING_STATE``, and finally the ``CONNECTED_STATE``, if the connection is successful. The handshake then starts automatically, and if it succeeds, the ``encrypted_handler`` is called to indicate the socket has entered the encrypted state and is ready for use.

Note that data can be written to the socket immediately after the return from ``connect_encrypted()`` (i.e. before the ``encrypted_handler`` is called) --- the data is queued in SslSocket until after the ``encrypted_handler`` is called.

Once encrypted, you use SslSocket as a regular TcpSocket. When the ``ready_read_handler`` is called, you can call the read functions (see :doc:`../stream`) to read decrypted data from the socket's internal buffer, and you can call one of the write functions to write data back to the peer. SslSocket will automatically encrypt the written data for you.

Properties
**********

SslSocket.encrypted
===================

Read only. True if the socket is encrypted.

An encrypted socket encrypts all data that is written by calling one of the write functions before the data is written to the network, and decrypts all incoming data as the data is received from the network, before you call one of the read functions.

SslSocket calls the ``encrypted_handler`` when it enters encrypted mode.

SslSocket.mode
==============

Read only. The current SSL mode for the socket; either ``UNENCRYPTED_MODE``, where the SslSocket behaves identically to a TcpSocket, or one of ``SSL_CLIENT_MODE`` or ``SSL_SERVER_MODE``, where the client is either negotiating or in encrypted mode.

When the mode changes, the ``mode_changed_handler`` is called.

.. list-table::
   :widths: 2 1 5
   :header-rows: 1
   
   * - Mode
     - Value
     - Description
   * - ``UNENCRYPTED_MODE``
     - 0
     - The socket is unencrypted. Its behaviour is identical to a TcpSocket.
   * - ``SSL_CLIENT_MODE``
     - 1
     - The socket is a client-side SSL socket. It is either already encrypted, or it is in the SSL handshake phase.
   * - ``SSL_SERVER_MODE``
     - 2
     - The socket is a server-side SSL socket. It is either already encrypted, or it is in the SSL handshake phase.

Methods
*******

SslSocket.new() -> SslSocket
============================

Create a new SslSocket.

SslSocket:connect_encrypted(hostname, port, openmode)
=====================================================

Attempts to make an encrypted connection to ``hostname`` on the given ``port``, using the given :ref:`stream-open-mode`. This is the equivalent of calling ``connect()`` to establish the connection, followed by a call to ``start_client_encryption()``.

SslSocket:start_client_encryption()
===================================

Starts a delayed SSL handshake for a client connection. This function can be called when the socket is in the ``CONNECTED_STATE`` but still in the ``UNENCRYPTED_MODE``. If it is not yet connected, or if it is already encrypted, this function has no effect.

Clients that implement STARTTLS functionality often make use of delayed SSL handshakes. Most other clients can avoid calling this function directly by using ``connect_encrypted()`` instead, which automatically performs the handshake.

Event handlers
**************

SslSocket.encrypted_handler
===========================

The handler has the following signature:

.. code-block:: lua

   function(socket)

The handler is called when the socket enters encrypted mode. After this handler has been called, the ``encrypted`` property will be true and all further transmissions on the socket will be encrypted.

SslSocket.mode_changed_handler
==============================

The handler has the following signature:

.. code-block:: lua

   function(socket, mode)

The handler is called when the socket SSL mode changes. ``mode`` is the new mode.
