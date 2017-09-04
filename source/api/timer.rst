Timer
#####

A repetitive or single-shot timer. To use a timer, assign a function to its ``timeout_handler`` and call ``start()`` - the handler will be called at constant intervals. The interval can be adjusted with the ``interval`` property.

Set the ``single_shot`` property to true if you only want the timer to call its ``timeout_handler`` once.

Properties
**********

Timer.interval
==============

The timeout interval in milliseconds.

Timer.remaining_time
====================

The remaining time in milliseconds.

Timer.active
============

True if the timer is running; otherwise false.

Timer.single_shot
=================

This property determines whether the timer is single-shot. A single-shot timer call its ``timeout_handler`` only once; non-single-shot timers call their ``timeout_handler`` every ``interval`` milliseconds.

Methods
*******

Timer.new() -> Timer
====================

Create a new timer.

Timer:start()
=============

Starts the timer.

Timer:stop()
============

Stops the timer.

Event handlers
**************

Timer.timeout_handler
=====================

The handler has the following signature:

.. code-block:: lua
   
   function(timer)

The handler is called each time the timer interval has elapsed.
