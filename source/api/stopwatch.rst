Stopwatch
#########

Use a stopwatch to find the elapsed time between events or actions.

Call ``start()`` to start the stopwatch. ``stop()`` halts the stopwatch at its current time; resume with a further call to ``start()``.

A new stopwatch will always start from zero. Call ``reset()`` to set the stopwatch time back to zero. The stopwatch will continue running after a call to ``reset()`` if it was already running.

While the stopwatch is running, its ``elapsed`` property will give its current time.

Properties
**********

Stopwatch.elapsed
=================

The current time in milliseconds.

Stopwatch.active
================

True if the stopwatch is running; otherwise false.

Methods
*******

Stopwatch.new() -> Stopwatch
============================

Create a new stopwatch.

Stopwatch:start()
=================

Starts or resumes the stopwatch.

Stopwatch:stop()
================

Stops the stopwatch, holding its current time.

Stopwatch:reset()
=================

Resets the stopwatch back to zero. If the stopwatch is active then the stopwatch will continue running from zero.
