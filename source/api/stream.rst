Stream
######

Stream is an abstract read/write interface for reading data from a source in chunks, as it becomes available. It's not used directly; objects such as ``TcpSocket`` expose its interface.

Properties
**********

Stream.is_open -> bool
======================

Read only. Returns ``true`` if the stream is open; otherwise returns ``false``. This property will be ``false`` if :ref:`stream-open-mode` is ``NOT_OPEN_MODE``.

.. _stream-open-mode:

Stream.open_mode
================

Read only. The mode in which the stream has been opened. Modes are static properties of Stream, e.g. ``Stream.READ_MODE``.

.. list-table::
   :widths: 2 1 5
   :header-rows: 1

   * - Mode
     - Value
     - Description
   * - ``NOT_OPEN_MODE``
     - 0x00
     - The stream is not open.
   * - ``READ_ONLY_MODE``
     - 0x01
     - The stream is open for reading.
   * - ``WRITE_ONLY_MODE``
     - 0x02
     - The stream is open for writing (implies truncate).
   * - ``READ_WRITE_MODE``
     - 0x03
     - The stream is open for reading and writing.
   * - ``APPEND_MODE``
     - 0x04
     - The stream is open in append mode so that all data is written to the end.
   * - ``TRUNCATE_MODE``
     - 0x08
     - If possible, the stream is truncated before it is opened. All earlier contents are lost.


Stream.bytes_available -> number
================================

Read only. The number of bytes that are available for reading. This property is commonly used with sequential streams to determine the number of bytes to allocate in a buffer before reading.

Stream.bytes_to_write -> number
===============================

Read only. For buffered streams, this property is the number of bytes waiting to be written. For streams with no buffer, ``bytes_to_write`` will always be 0.

Stream.at_end -> bool
=====================

Read only. True if the current read position is at the end of the device, i.e. there's no more data to read.

Stream.pos -> number
====================

Read only. For random-access streams, this property is the position that data is written to or read from. For sequential devices or closed devices, where there is no concept of a "current position", ``pos`` will always be 0.

Stream.readable -> bool
=======================

Read only. True if this stream is open for reading.

Stream.writeable -> bool
=======================

Read only. True if this stream is open for writing.

Stream.sequential -> bool
=========================

Read only. True if this stream is sequential. Sequential streams, as opposed to random-access streams, have no concept of a start, an end, a size, or a current position, and they do not support seeking. You can only read from the stream when it reports that data is available. The most common example of a sequential stream is a network socket.

Regular files, on the other hand, do support random access. They have both a size and a current position, and they also support seeking backwards and forwards in the data stream. Regular files are non-sequential.

Stream.can_read_line -> bool
============================

Read only. True if a complete line of data can be read from the stream; otherwise returns ``false``.

Note that this property is always ``false`` for unbuffered streams, which have no way of determining what can be read.

Stream.error_string -> string
=============================

Read only. A human-readable description of the last error that occurred.

Methods
*******

Stream:read_string(max) -> string
=================================

Returns data up to ``max`` length as a string, or all data available if ``max`` not given. Returned string will be empty if there are no bytes available to read.

Stream:read_string_async(max, callback)
=======================================

Asynchronous version of ``Stream:read_string()``, which must be used when outside the stream's event handlers.

The callback has the following signature:

.. code-block:: lua

   function(stream, data, error)

``data`` will be as for the return value of ``Stream:read_string()``. If an internal error occurs then ``error`` will be non-nil.

Stream:read_bytes(max) -> table
===============================

Returns data up to ``max`` bytes as a numerically-indexed table of bytes (stored as numbers), or all data available if ``max`` not given. Returned table will be empty if there are no bytes available to read.

Stream:read_bytes_async(max, callback)
======================================

Asynchronous version of ``Stream:read_bytes()``, which must be used when outside the stream's event handlers.

The callback has the following signature:

.. code-block:: lua

   function(stream, data, error)

``data`` will be as for the return value of ``Stream:read_bytes()``. If an internal error occurs then ``error`` will be non-nil.

Stream:read_line_string(max) -> string
======================================

Reads a line from the stream as a string but not more than max characters. Returned string will be empty if there are no bytes available to read.

Stream:read_line_string_async(max, callback)
============================================

Asynchronous version of ``Stream:read_line_string()``, which must be used when outside the stream's event handlers.

The callback has the following signature:

.. code-block:: lua

   function(stream, data, error)

``data`` will be as for the return value of ``Stream:read_line_string()``. If an internal error occurs then ``error`` will be non-nil.

Stream:read_line_bytes(max) -> table
====================================

Reads a line from the stream as a numerically-indexed table of bytes (stored as numbers), but not more than ``max`` characters. Returned table will be empty if there are no bytes available to read.

Stream:read_line_bytes_async(max, callback)
===========================================

Asynchronous version of ``Stream:read_line_bytes()``, which must be used when outside the stream's event handlers.

The callback has the following signature:

.. code-block:: lua

   function(stream, data, error)

``data`` will be as for the return value of ``Stream:read_line_bytes()``. If an internal error occurs then ``error`` will be non-nil.

Stream:read_all_string() -> string
==================================

Reads all the data available as a string.

Stream:read_all_string_async(callback)
======================================

Asynchronous version of ``Stream:read_all_string()``, which must be used when outside the stream's event handlers.

The callback has the following signature:

.. code-block:: lua

   function(stream, data, error)

``data`` will be as for the return value of ``Stream:read_all_string()``. If an internal error occurs then ``error`` will be non-nil.

Stream:read_all_bytes() -> table
================================

Reads all the data available as a numerically-indexed table of bytes (stored as numbers).

Stream:read_all_bytes_async(callback)
=====================================

Asynchronous version of ``Stream:read_all_bytes()``, which must be used when outside the stream's event handlers.

The callback has the following signature:

.. code-block:: lua

   function(stream, data, error)

``data`` will be as for the return value of ``Stream:read_all_bytes()``. If an internal error occurs then ``error`` will be non-nil.

Stream:write_string(string) -> number
=====================================

Writes ``string`` to the underlying device, returning the number of bytes written.

Stream:write_string_async(string, callback)
===========================================

Asynchronous version of ``Stream:write_string()``, which must be used when outside the stream's event handlers.

The callback has the following signature:

.. code-block:: lua

   function(stream, bytes_written, error)

``bytes_written`` will be as for the return value of ``Stream:write_string()``. If an internal error occurs then ``error`` will be non-nil.

Stream:write_bytes(table) -> number
===================================

Writes the values in the table to the underlying device as bytes, returning the number of bytes written.

Stream:write_bytes_async(table, callback)
=========================================

Asynchronous version of ``Stream:write_bytes()``, which must be used when outside the stream's event handlers.

The callback has the following signature:

.. code-block:: lua

   function(stream, bytes_written, error)

``bytes_written`` will be as for the return value of ``Stream:write_bytes()``. If an internal error occurs then ``error`` will be non-nil.

Stream:peek_string(max) -> string
=================================

Reads at most ``max`` bytes from the stream without side effects (i.e. if you call ``read()`` after ``peek()``, you will get the same data). Returns the data read as a string. If the returned string is empty then it may mean that no data was available for peeking or that an error occurred.

Stream:peek_string_async(max, callback)
=======================================

Asynchronous version of ``Stream:peek_string()``, which must be used when outside the stream's event handlers.

The callback has the following signature:

.. code-block:: lua

   function(stream, data, error)

``data`` will be as for the return value of ``Stream:peek_string()``. If an internal error occurs then ``error`` will be non-nil.

Stream:peek_bytes(max) -> table
===============================

Reads at most ``max`` bytes from the stream, without side effects (i.e. if you call ``read()`` after ``peek()``, you will get the same data). Returns the data read as a numerically-indexed table of bytes (stored as numbers). If the returned table is empty then it may mean that no data was available for peeking or that an error occurred.

Stream:peek_bytes_async(max, callback)
======================================

Asynchronous version of ``Stream:peek_bytes()``, which must be used when outside the stream's event handlers.

The callback has the following signature:

.. code-block:: lua

   function(stream, data, error)

``data`` will be as for the return value of ``Stream:peek_bytes()``. If an internal error occurs then ``error`` will be non-nil.

Stream:seek(pos) -> bool
========================

For random-access streams, this function sets the current position to ``pos``, returning true on success, or false if an error occurred. For sequential devices, false will be returned.

Stream:seek_async(pos, callback)
================================

Asynchronous version of ``Stream:seek()``, which must be used when outside the stream's event handlers.

The callback has the following signature:

.. code-block:: lua

   function(stream, result, error)

``result`` will be as for the return value of ``Stream:seek()``. If an internal error occurs then ``error`` will be non-nil.

Stream:reset() -> bool
======================

For random-access streams, this function sets the current position to the start of the input, returning true on success, or false if an error occurred. For sequential devices, false will be returned.

Stream:reset_async(callback)
============================

Asynchronous version of ``Stream:reset()``, which must be used when outside the stream's event handlers.

The callback has the following signature:

.. code-block:: lua

   function(stream, result, error)

``result`` will be as for the return value of ``Stream:reset()``. If an internal error occurs then ``error`` will be non-nil.

Stream:open(mode) -> bool
=========================

Returns true if the stream could be opened under the given ``mode`` (see :ref:`stream-open-mode`).

Stream:open_async(mode, callback)
=================================

Asynchronous version of ``Stream:open()``, which must be used when outside the stream's event handlers.

The callback has the following signature:

.. code-block:: lua

   function(stream, result, error)

``result`` will be as for the return value of ``Stream:open()``. If an internal error occurs then ``error`` will be non-nil.

Stream:close()
==============

Close the stream for reading/writing.

Event handlers
**************

Stream.ready_read_handler
=========================

The handler has the following signature:

.. code-block:: lua

   function(stream)

The handler is called when new data is available to read.

Stream.bytes_written_handler
============================

The handler has the following signature:

.. code-block:: lua

   function(stream, bytes)

The handler is called every time data is written to the underlying device.

Stream.about_to_close_handler
=============================

The handler has the following signature:

.. code-block:: lua

   function(stream)

The handler is called when any of the underlying resources (e.g. file descriptor, serial port, TCP/IP) are about to be closed. There may still be data to read.
