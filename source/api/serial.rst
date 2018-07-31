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

Usage Example
*************

config.json
===========

.. code-block:: javascript

    {
        "instanceProperties": [
            {
                "name": "Serial Port",
                "type": "serial"
            }
        ]
    }

main.lua
========

.. code-block:: lua

    instance.initialise = function()

        serialConnection = iomodules.Serial.new() -- Create Serial Port
        serialConnection:bind(instance:property("Serial Port")) -- Bind to specified port
        serialConnection:open(iomodules.Stream.READ_WRITE_MODE) -- Open port

        serialConnection.ready_read_handler = function(stream)
        
            local incomingMessage = stream:read_all_string()

            -- Do something with the incoming message

        end

        serialConnection:write_string("Message to send")

    end