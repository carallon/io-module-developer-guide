Analog Input
############

The AnalogInput object allows IO modules to hook up to the analog inputs on controllers and remote devices and to be notified about changes to an input's level.

Each AnalogInput object can listen to one analog input on a controller or remote device. Create more AnalogInput objects as required to listen for changes on multiple inputs.

Properties
**********

AnalogInput.level -> number
===========================

Read only. The current level of the analog input in the range 0-100, representing a fraction through the voltage range configured for the input in the Designer project.

Will return ``nil`` if the AnalogInput object is not currently bound to an input.

The level of any analog input can also be queried through the :doc:`../../guide/controller-api`.

Methods
*******

AnalogInput.new()
=================

Create a new AnalogInput object.

AnalogInput:bind(input) -> bool
===============================

Binds the analog input object exclusively to an analog ``input``, where ``input`` is an object obtained from a user property of type ``"analogInput"`` (see :ref:`resource-property-types`).

Returns false if the input is already in use by another IO module.

AnalogInput:close()
===================

Stops listening to the analog input, releasing this resource for use elsewhere. ``bind()`` can now be called to bind the object to another input.

Event handlers
**************

AnalogInput.level_changed_handler
=================================

The handler has the following signature:

.. code-block:: lua

   function(analog_input, level)

This handler is called when the level of the analog input changes. The ``level`` argument is a number (integer) between 0 and 100. It represents a fraction through the voltage range configured for the input in the Designer project.
