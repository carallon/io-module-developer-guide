Payload
#######

The Payload object is a container for sending data over HTTP, e.g. in the ``http.request()`` method.

Set the payload's data via the appropriate property, e.g. use the ``string`` property to set the payload data from a string. The ``type`` property reflects the type of data that the Payload is carrying.

Properties
**********

Payload.type
============

Read only. The current type of the payload. Current types are:

* ``STRING``
* ``BYTE_TABLE``

Payload.string
==============

String data of the payload (will be empty if the current type of the payload object is not ``STRING``).

Payload.bytes
=============
Byte table data of the payload (will be nil if the current type of the payload object is not ``BYTE_TABLE``).

Methods
*******

Payload.new() -> Payload
========================

Create a new payload.
