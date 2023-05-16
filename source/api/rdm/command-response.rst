RDM command response
####################

The RDM Command Response object is returned from RDM command (GET or SET) requests. It includes event handlers for the lifecycle of a discovery request.

Event handlers
**************

started_handler
===============

The handler has the following signature:

.. code-block:: lua

   function(command_response)

The handler is called to indicate that the process of sending the command has started.

rx_handler
==========

The handler has the following signature:

.. code-block:: lua

    function(command_response, payload)

This handler is called to indicate that response data has been received from the command. It includes:

* The response object

* payload - the data received. The type of the payload depends on the command, see the :ref:`table of standard commands<standard-rdm-pids>`.

finished_handler
================

The handler has the following signature:

.. code-block:: lua

    function(command_response)

This handler is called to indicate that the RDM command has finished.

error_handler
=============

The handler has the following signature:

.. code-block:: lua

    function(command_response, error, error_message)

If sending the command failed due to errors, error_handler will be called. The error code will be one of those enumerated below.


.. list-table::
   :widths: 2 5
   :header-rows: 1

   * - Error
     - Description
   * - ``rdm.TIMEOUT``
     - The discovery attempt timed out with no responses.
   * - ``rdm.COLLISION``
     - There was an unexpected RDM collision.
   * - ``rdm.INTERNAL_ERROR``
     - An internal error prevented discovery completing.
   * - ``rdm.USER_CANCELLED``
     - Other user actions caused the discovery attempt to be cancelled.
   * - ``rdm.LENGTH_INVALID``
     - The reported length of the returned RDM data was invalid.
   * - ``rdm.CHECKSUM_INVALID``
     - The checksum of the returned RDM data was invalid.
   * - ``rdm.FORMAT_ERROR``
     - The responder cannot interpret the request as controller data was not formatted correctly.
   * - ``rdm.HARDWARE_FAULT``
     - The responder cannot comply due to an internal hardware fault.
   * - ``rdm.PROXY_REJECT``
     - Proxy is not the RDM line master and cannot comply with message.
   * - ``rdm.WRITE_PROTECT``
     - SET Command normally allowed but being blocked currently.
   * - ``rdm.UNSUPPORTED_COMMAND_CLASS``
     - Not valid for Command Class attempted. May be used where GET allowed but SET is not supported.
   * - ``rdm.DATA_OUT_OF_RANGE``
     - Value for given Parameter out of allowable range or not supported.
   * - ``rdm.BUFFER_FULL``
     - Buffer or Queue space currently has no free space to store data.
   * - ``rdm.PACKET_SIZE_UNSUPPORTED``
     - Incoming message exceeds buffer capacity.
   * - ``rdm.SUB_DEVICE_OUT_OF_RANGE``
     - Sub-Device is out of range or unknown.
   * - ``rdm.PROXY_BUFFER_FULL``
     - The proxy buffer is full and can not store any more Queued Message or Status Message responses.
