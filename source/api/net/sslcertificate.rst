net.SslCertificate
##################

Represents an certificate for use in SSL/TLS handshakes. Can be created from a string in PEM or DER format.

Properties
**********

SslCertificate.null -> bool
===========================

Read only. False if the certificate was not loaded successfully.

Methods
*******

SslCertificate.new(encoded_certificate, format) -> SslCertificate
======================================

Creates a new ``SslCertificate`` from the string ``encoded_certficate`` using the format given by ``format``.

``encoded_certificate`` is an encoded certificate whose encoding must match ``format``.

If specified, ``format`` must be one of the values in ``net.SslEncoding``. If ``format`` is not specified, ``net.SslEncoding.PEM`` is assumed.
