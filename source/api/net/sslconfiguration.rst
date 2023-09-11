net.SslConfiguration
####################

Represents the configuration of an SSL connection.

Properties
**********

SslConfiguration.local_certificate -> SslCertificate
====================================================

The ``net.SslCertificate`` to be sent to the server in the Client Certificate message.

SslConfiguration.local_certificate_chain -> table
=================================================

The table of ``net.SslCertificate`` establishing the chain of trust for the ``SslConfiguration.local_certificate``.


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

SslConfiguration.add_ca_certificate(SslCertificate)
===================================================

Adds ``net.SslCertificate`` to this configuration's CA certificate database.
The certificate database must be set prior to the SSL handshake.

The CA certificate database is used by the socket during the handshake phase to validate the peer's certificate.
