RDM commands
############

The RDM API allows you to send RDM GET or SET commands to a device once it has been discovered.

There are two options to send SET or GET commands:

Pre-Defined PIDs
================

The pre-defined PIDs are listed below. Pre-Defined, in this context just means a subset of PIDs we support by default. When you perform a GET or SET on these PIDs, you supply or will be returned a Lua table of values as described below.

Custom PIDs
===========

For PIDs which are not in our pre-defined list, they can still be SET/GET using the commands below, but the payload must be the PID data packed into a Lua string using the format from the standard.

Similarly, responses will be returned as raw data packed into a Lua string, and it is the responsibility of the IO module to correctly unpack data according to the RDM standard. 

Functions
*********

rdm.get(uid, key, pid, payload) -> response
===========================================

This function initiates a GET of an RDM property. It takes the following parameters:

* uid - An RDM :doc:`Device ID<device-id>`, created using one of the device ID functions.

* key - A :doc:`Universe Key<universe-key>`, created using one of the universe key functions.

* pid - The PID to GET. These are defined by the RDM standard. Either one of the pre-defined PIDs below, or an arbitrary PID value as an integer.

* payload - The payload for the RDM request, if required. The payload should be as specified in the table below for pre-defined PID requests. For other PIDs, it should be a lua string with the data packed appropriately according to the RDM standard.

It returns a :doc:`command-response object<command-response>`.

rdm.set(uid, key, pid, payload) -> response
===========================================

This function initiates a SET of an RDM property. It takes the following parameters:

* uid - An RDM :doc:`Device ID<device-id>`, created using one of the device ID functions.

* key - A :doc:`Universe Key<universe-key>`, created using one of the universe key functions.

* pid - The PID to GET. These are defined by the RDM standard. Either one of the pre-defined PIDs below, or an arbitrary PID value as an integer.

* payload - The payload for the RDM request, if required. The payload should be as specified in the table below for pre-defined PID requests. For other PIDs, it should be a lua string with the data packed appropriately according to the RDM standard.

It returns a :doc:`command-response object<command-response>`.

.. _standard-rdm-pids:

Pre-defined RDM parameters
**************************

.. list-table::
   :widths: 5 2 10
   :header-rows: 1

   * - RDM Parameter
     - Value
     - Expected Parameters
   * - ``rdm.COMMS_STATUS``
     - 0x0015
     - Setting the parameter clears the statistics. Getting the parameter returns a lua table with values for ``short_message``, ``length_mismatch`` and ``checksum_fail``.
   * - ``rdm.STATUS_MESSAGE``
     - 0x0030
     - Get only. Getting the parameter returns a lua table with values for ``subdevice_id``, ``status_type``, ``message_id``, ``data_1`` and ``data_2`` for each message returned.
   * - ``rdm.SUPPORTED_PARAMETERS``
     - 0x0050
     - Get only. Returns a lua table of integer values for the supported parameters.
   * - ``rdm.PARAMETER_DESCRIPTION``
     - 0x0051
     - Get only. The integer parameter you want the description of must be passed into the get call. Returns a lua table with values for ``pid``, ``pdl_size``, ``data_type``, ``command_class``, ``type``, ``unit``, ``prefix``, ``min_valid_value``, ``max_valid_value`` and ``description``
   * - ``rdm.DEVICE_INFO``
     - 0x0060
     - Get only. Returns a device info structure for the device.
   * - ``rdm.DEVICE_MODEL_DESCRIPTION``
     - 0x0080
     - Get only. Returns the device model reported by the device, as a string.
   * - ``rdm.MANUFACTURER_LABEL``
     - 0x0081
     - Get only. Returns the manufacturer name reported by the device, as a string.
   * - ``rdm.DEVICE_LABEL``
     - 0x0082
     - Get or Set the user-assigned label for the device, as a string.
   * - ``rdm.FACTORY_DEFAULTS``
     - 0x0090
     - Set only. Resets the device to factory defaults.
   * - ``rdm.SOFTWARE_VERSION_LABEL``
     - 0x00c0
     - Get only. Returns the software version label reported by the device, as a string.
   * - ``rdm.BOOT_SOFTWARE_VERSION_ID``
     - 0x00c1
     - Get only. Returns the software version of the bootloader as reported by the device, as an integer.
   * - ``rdm.BOOT_SOFTWARE_VERSION_LABEL``
     - 0x00c2
     - Get only. Returns the software version of the bootloader as reported by the device, as a string.
   * - ``rdm.DMX_PERSONALITY``
     - 0x00e0
     - Get or Set. Gets or sets the DMX personality the device is currently using, as an integer.
   * - ``rdm.DMX_PERSONALITY_DESCRIPTION``
     - 0x00e1
     - Get only. Returns a string description of the integer DMX personality identifier passed with the command.
   * - ``rdm.DMX_START_ADDRESS``
     - 0x00f0
     - Get or Set. Gets or Sets the current DMX start address of the device.
   * - ``rdm.SLOT_INFO``
     - 0x0120
     - Get only. Returns a lua table of slots. For each slot it includes values for ``slot_offset``, ``slot_type`` and ``slot_label_id``.
   * - ``rdm.SLOT_DESCRIPTION``
     - 0x0121
     - Get only. Returns a string description for the slot number provided with the Get command.
   * - ``rdm.SENSOR_DEFINITION``
     - 0x0200
     - Get only. Returns a lua table for the requested sensor, including values for ``sensor_number_requested``, ``type``, ``unit``, ``prefix``, ``range_minimum_value``, ``range_maximum_value``, ``normal_minimum_value``, ``normal_maximum_value``, ``recorded_value_support``, ``description``
   * - ``rdm.SENSOR_VALUE``
     - 0x0201
     - Get only. Returns a value for the requested sensor, including values for ``sensor_number_requested``, ``present_value``, ``lowest_detected_value``, ``highest_detected_value``, ``recorded_value``.
   * - ``rdm.LAMP_HOURS``
     - 0x0401
     - Get only. Returns the lamp hours for the device, as an integer.
   * - ``rdm.LAMP_STATE``
     - 0x0403
     - Get only. Returns the lamp state for the device, as an integer. See table A-8 of the RDM standard for possible values.
