net.SslKey
#############

Represents a public/private key used by an SSL socket or server.

Properties
**********

SslKey.null -> bool
===================

Read only. False if the key was not loaded successfully.

SslKey.type
===========

Read only. The key's type.

.. list-table::
    :widths: 2 1 5
    :header-rows: 1

    * - Type
      - Value
      - Description
    * - ``PUBLIC``
      - 0
      - The key is public.
    * - ``PRIVATE``
      - 1
      - The key is private.

SslKey.algorithm
================

Read only. The key's algorithm.

.. list-table::
    :widths: 2 1 5
    :header-rows: 1

    * - Algorithm
      - Value
      - Description
    * - RSA
      - 0
      - The RSA algorithm.
    * - DSA
      - 1
      - The DSA algorithm.
    * - EC
      - 2
      - The Elliptic Curve algorithm.

Methods
*******

SslKey.new(encoded_key, algorithm, format, type) -> SslKey
======================

Creates a new SslKey from the string  ``encoded_key``.

``encoded_key`` is an encoded public or private key whose encoding must match ``format``.

``algorithm`` specifies the key algorithm. It must be specified as one of ``SslKey.RSA``, ``SslKey.DSA`` or ``SslKey.EC``.

``format`` can be one of the values in ``net.SslEncoding``. If not given, ``net.SslEncoding.PEM`` is assumed.

``type`` can be either ``SslKey.PUBLIC`` or ``SslKey.PRIVATE``. If not given, ``SslKey.PRIVATE`` is assumed.
