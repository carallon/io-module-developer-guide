net.SslConfiguration
####################

Represents the configuration of an SSL connection.

Properties
**********

SslConfiguration.local_certificate -> SslCertificate
====================================================

The certificate to be sent to the server in the Client Certificate message.

.. _ssl-configuration-peer-verify-mode:

SslConfiguration.peer_verify_mode
=================================

Sets the peer verify mode, which decides whether the underlying :doc:`./sslsocket` should request a certificate from the peer (i.e. the client requests a certificate from the server, or a server requests a certificate from the client), and whether it should require that this certificate is valid.

The default mode is ``net.SslSocket.AUTO_VERIFY_PEER``, which tells the ``SslSocket`` to use ``VERIFY_PEER`` for clients and ``QUERY_PEER`` for servers.

See :ref:`peer verify mode<ssl-socket-peer-verify-mode>` in ``net.SslSocket`` for more details.

SslConfiguration.private_key -> SslKey
======================================

The private key corresponding to local_certificate.

Methods
*******

SslConfiguration.new() -> SslConfiguration
==========================================

Creates a new SslConfiguration with the default configuration.
