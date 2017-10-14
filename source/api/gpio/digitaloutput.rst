gpio.DigitalOutput
##################

The DigitalOutput object allows IO modules to hook up to the digital outputs on remote devices and change their state.

Each DigitalOutput object can control one digital output on a remote device. Create more DigitalOutput objects as required to control multiple outputs.

Properties
**********

DigitalOutput.state -> bool
===========================

Read/write. The current state of the digital output --- true if the digital output is on (relay closed) and false if the digital output is off (relay open).

Will return ``nil`` if the DigitalOutput object is not currently bound to an output.

Methods
*******

DigitalOutput.new()
===================

Create a new DigitalOutput object.

DigitalOutput:bind(output) -> bool
==================================

Binds the digital output object exclusively to a digital ``output``, where ``output`` is an object obtained from a user property of type ``"digitalOutput"`` (see :ref:`resource-property-types`).

Returns false if the output is already in use by another IO module.

DigitalOutput:close()
=====================

Stops controlling the digital output, releasing this resource for use elsewhere. ``bind()`` can now be called to bind the object to another output.

