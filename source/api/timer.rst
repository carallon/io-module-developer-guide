Timer
#####

A repetitive or single-shot timer. To use a timer, assign a function to its ``timeout_handler`` and call ``start()`` - the handler will be called at constant intervals. The interval can be adjusted with the ``interval`` property.

A timer with an interval of 0 will call its ``timeout_handler`` on every controller refresh.

Set the ``single_shot`` property to true if you only want the timer to call its ``timeout_handler`` once.

Properties
**********

Timer.interval
==============

Read/write. The timeout interval in milliseconds.

Changing the interval while the timer is active will restart the timer with the new interval.

Setting this property to 0 will result in the ``timeout_handler`` being called on every controller refresh. The timer will become inactive if this property is set to a negative value and will remain so until a non-negative value is set.

Timer.remaining_time
====================

Read only. The remaining time in milliseconds.

Timer.active
============

Read only. True if the timer is running; otherwise false.

Timer.single_shot
=================

Read/write. This property determines whether the timer is single-shot. A single-shot timer call its ``timeout_handler`` only once; non-single-shot timers call their ``timeout_handler`` every ``interval`` milliseconds.

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
