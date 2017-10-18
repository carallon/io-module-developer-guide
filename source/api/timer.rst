Timer
#####

A repetitive or single-shot timer. To use a timer, assign a function to its ``timeout_handler`` and call ``start()`` - the handler will be called at constant intervals. The interval can be adjusted with the ``interval`` property.

A timer with an interval of 0 will timeout on every controller refresh.

A timer with a negative interval will be disabled until the interval is set to a non-negative value.

Set the ``single_shot`` property to true if you only want the timer to call its ``timeout_handler`` once.

Properties
**********

Timer.interval
==============

The timeout interval in milliseconds. If this is set to a non-negative value while the timer is active, the timer will restart. If this is set to a negative value while the timer is active, the timer will stop.

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

Starts the timer. If the timer is already active it will be restarted.

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
