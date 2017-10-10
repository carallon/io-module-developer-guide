Serial I/O
##########

**Implements:** :doc:`stream`

The Serial object is used to send & receive data over the serial interface of a controller or remote device. It implements the readable/writeable stream interface.

Methods
*******

Serial.new() -> Serial
======================

Creates a new Serial object.

Serial:bind(interface) -> bool
==============================

Binds the serial object exclusively to a serial ``interface``, where ``interface`` is an object obtained from a user property of type ``"serial"`` (see :ref:`resource-property-types`).

Serial:close()
==============

Closes the serial interface, releasing this resource for use elsewhere. ``bind()`` can now be called to bind the object to another interface.
