Digital Input
#############

The DigitalInput object allows IO modules to hook up to the digital inputs on controllers and remote devices and to be notified about changes to an input's state.

Each DigitalInput object can listen to one digital input on a controller or remote device. Create more DigitalInput objects as required to listen for changes on multiple inputs.

Properties
**********

DigitalInput.state -> bool
==========================

Read only. The current state of the digital input --- true if the digital input is 'high' and false if the digital input is 'low'. The high and low thresholds of digital inputs are configured in the Designer project.

Will return ``nil`` if the DigitalInput object is not currently bound to an input.

The state of any digital input can also be queried through the :doc:`../../guide/controller-api`. 

DigitalInput.held -> bool
=========================

Read only. Whether the input has been in its current state for longer than the held timeout. The held timeout is configured for each controller and remote device in the Designer project.

Will return ``nil`` if the DigitalInput object is not currently bound to an input.

Methods
*******

DigitalInput.new()
==================

Create a new DigitalInput object.

DigitalInput:bind(input) -> bool
================================

Binds the digital input object exclusively to a digital ``input``, where ``input`` is an object obtained from a user property of type ``"digitalInput"`` (see :ref:`resource-property-types`).

Returns false if the input is already in use by another IO module.

DigitalInput:close()
====================

Stops listening to the digital input, releasing this resource for use elsewhere. ``bind()`` can now be called to bind the object to another input.

Event handlers
**************

DigitalInput.state_changed_handler
==================================

The handler has the following signature:

.. code-block:: lua

   function(digital_input, state)

This handler is called when the state of the digital input changes. The ``state`` argument is a boolean --- it's true if the digital input is 'high' and false if the digital input is 'low'. The high and low thresholds of digital inputs are configured in the Designer project.

DigitalInput.held_handler
=========================

The handler has the following signature:

.. code-block:: lua

   function(digital_input, state)

The handler is called when the digital input has been held in the ``state`` for the held timeout. The held timeout is configured for each controller and remote device in the Designer project.

DigitalInput.repeat_handler
===========================

The handler has the following signature:

.. code-block:: lua

   function(digital_input, state)

The handler is called at regular intervals while the digital input is held in the ``state`` beyond the held timeout. The repeat interval and held timeout are configured for each controller and remote device in the Designer project.
