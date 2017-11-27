Alarm
#####

A real time alarm to trigger a one-off or repeating function. The event condition is set through the ``set_time`` property.

The alarm can be cancelled at any time with ``cancel()``.

Properties
**********

Alarm.condition_valid
============

Read only. True if the alarm's event condition is valid, otherwise false. Alarms for which this is false cannot be started.

Alarm.active
============

Read only. True if the alarm will call the ``alarm_handler`` when the next event condition occurs; otherwise false.

Alarm.single_shot
=================

This property determines whether the alarm is single-shot. A single-shot alarm will call its ``alarm_handler`` only once; repeating alarms call their ``alarm_handler`` every time their event occurs.

Methods
*******

Alarm.new() -> Alarm
====================

Create a new alarm.

Alarm:cancel()
==============

Cancels the alarm. The ``alarm_handler`` will not be called next time the event condition is matched.

Alarm:start()
==============

If ``condition_valid`` is true, starts or resumes the alarm; the ``alarm_handler`` will be called next time the event condition is matched. If ``condition_valid`` is false, the alarm is not started and an error is logged. 

Alarm:set_time(hour, minute, second)
==============

Sets the alarm's event condition to match the time ``hour``:``minute``:``second``. To be valid, the ``hour``, ``minute`` and ``second`` arguments must be in the range 0-23, 0-59 and 0-59 respectively. If all arguments are valid, ``condition_valid`` becomes true. If at least one argument is invalid, the alarm will be canceled and ``condition_valid`` will be false until a valid time is set.

Event handlers
**************

Alarm.alarm_handler
===================

The handler has the following signature:

.. code-block:: lua
   
   function(alarm)

The handler is called each time the alarm event condition is matched.
