net.ICMP
########

ICMP (Internet Control Message Protocol) is a supporting protocol in the Internet protocol suite. It is used by network devices, including routers, to send error messages and operational information indicating success or failure when communicating with another IP address.

Every device (such as an intermediate router) forwarding an IP datagram first decrements the time to live ``ttl`` field in the IP header by one. If the resulting TTL is 0, the packet is discarded.

Properties
**********

ICMP.error
==========

Read only. The type of error that last occurred. Errors are static properties of ICMP, e.g. ``ICMP.HOST_NOT_FOUND``.

.. list-table::
   :widths: 2 1 5
   :header-rows: 1

   * - Error
     - Value
     - Description
   * - ``HOST_NOT_FOUND``
     - 1
     - The hostname could not be resolved, or didn't not resolve to a useable IPv4 address.
   * - ``UNKNOWN_ERROR``
     - 2
     - An unidentified error occurred.

ICMP.error_string -> string
===========================

Read only. A human-readable description of the last error that occurred.

Methods
*******

ICMP.new(ttl) -> ICMP
=====================

Creates a new ICMP object.

ICMP:send_ping(hostname, timeout) -> bool
=========================================

Send a ping echo request to ``hostname`` with a reply timeout of ``timeout``.

The ``hostname`` may be an IP address in string form (e.g. "43.195.83.32"), or it may be a host name (e.g. "www.example.com").

``timeout`` is in given in milliseconds.

Returns true if successful; otherwise the ``error`` property will be set accordingly.

Event handlers
**************

ICMP.ping_reply_handler
=======================

The handler has the following signature:

.. code-block:: lua

   function(icmp, ip, time, ttl, sequence)

The handler is called on reception of a ping reply (ICMP echo response).

The reply ``ip`` is given as an integer, this can be converted to dotted decimal format using ``controller.Variant``

``time`` is the number of milliseconds taken for the reply to arrive.

The ``sequence`` number is automatically increased, starting from 0, on each call of ``send_ping()``.

ICMP.timeout_handler
====================

The handler has the following signature:

.. code-block:: lua

   function(icmp, timeout)

The handler is called after no ping reply (ICMP echo response) following a interval of ``timeout`` milliseconds.

ICMP.error_handler
=======================

The handler has the following signature:

.. code-block:: lua

   function(icmp, error, error_string)

The handler is called when an error occurs. The error types are given with the ``error`` property. The ``error_string`` property will give a human-readable description of the error.
