RDM discovery response
######################

The RDM Discovery Response object is returned from RDM discovery requests. It includes both data and event handlers for the lifecycle of a discovery request.

Event handlers
**************

started_handler
===============

The handler has the following signature:

.. code-block:: lua

   function(response)

The handler is called to indicate that the discovery process requested has started.

device_found_handler
====================

The handler has the following signature:

.. code-block:: lua

    function(discovery_response, device_info, fixture_number)

This handler is called to indicate that a response has been received from discovery. It includes:

* The response object
* device_info - a table of information about the discovered device, see below
* fixture_number - If the fixture corresponds to a patched fixture in Designer, the fixture number - if not, this will be ``nil``

finished_handler
================

The handler has the following signature:

.. code-block:: lua

    function(discovery_response, error, error_message)

This handler is called to indicate that the RDM discovery process has finished.

If the process completed without errors, error will be ``nil``.

If there are errors, error will be an integer error code:

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


error_handler
=============

The handler has the following signature:

.. code-block:: lua

    function(response, error, error_message)

If the discovery process failed due to errors, error_handler will be called. The error code will be one of those enumerated above.


device_info object
==================

The device info object contains the parameters from RDM discovery that describe a device. The members are:


.. list-table::
   :widths: 5 2 10
   :header-rows: 1

   * - Parameter
     - Type
     - Description
   * - ``fixture_number``
     - integer
     - The user number assigned to this fixture in designer, or 0 if not associated with a fixture
   * - ``rdm_uid``
     - An :doc:`RDM device ID<device-id>` object
     - The RDM Unique ID for the fixture.
   * - ``output_device``
     - string
     - The :doc:`universe key ID<universe-key>` of the device on which this RDM device was discovered.
   * - ``rdm_protocol_version``
     - integer
     - The RDM protocol version the device supports, packed into a single 16-bit value as described in the RDM standard.
   * - ``model_id``
     - integer
     - The RDM Model ID of the device.
   * - ``product_category``
     - integer
     - The RDM Product Category of the device, packed into a single 16-bit value as described in the RDM standard.
   * - ``software_version``
     - integer
     - The integer software version identifier of the device.
   * - ``footprint``
     - integer
     - The number of DMX addresses used by the device.
   * - ``address``
     - integer
     - The current DMX address of the device.
   * - ``current_personality``
     - integer
     - The current DMX personality configured to the device.
   * - ``sub_device_count``
     - integer
     - The number of subdevices the device reports.
   * - ``personality_count``
     - integer
     - the number of personalities the device reports.
   * - ``sensor_count``
     - integer
     - The number of sensors the device reports.
