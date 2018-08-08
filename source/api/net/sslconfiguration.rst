net.SslConfiguration
####################

Represents the configuration of an SSL connection.

Properties
**********

SslConfiguration.local_certificate -> SslCertificate
====================================================

The certificate to be sent to the server in the Client Certificate message.

SslConfiguration.private_key -> SslKey
======================================

The private key corresponding to local_certificate.

Methods
*******

SslConfiguration.new() -> SslConfiguration
==========================================

Creates a new SslConfiguration with the default configuration.
