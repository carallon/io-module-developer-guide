RDM device ID
#############

An RDM device ID represents the unique device ID from the RDM standard.

A device ID consists of two parts, a manufacturer identifier (16 bits) and a device identifier (32 bits).

They are often combined with a subdevice identifier (16-bits), where a subdevice is a logical device within another device - for example a dimmer circuit within a dimmer pack.

These device IDs are often represented in text as mmmm:dddddddd:ssss where:

* ``mmmm`` is the manufacturer ID in hexadecimal, 0-padded
* ``dddddddd`` is the device ID in hexadecimal, 0-padded
* ``ssss`` is the subdevice identifier, in hexadecimal, 0-padded, and may be omitted when zero, i.e. addressing the root device

The RDM namespace provides the deviceId methods to create and manipulate these IDs.

Methods
*******

rdm.DeviceId.new_in_parts(manufacturer, device, subdevice) -> deviceId
======================================================================

Creates a new Device ID using a specified manufacturer ID, device ID and subdevice ID.

To address the root device, pass in a subdevice ID of 0.


rdm.DeviceId.new_whole(id) -> deviceId
======================================

Creates a new deviceId using a single 64-bit integer value, representing a combination of the manufacturer and device ID.

The Subdevice value will be set to zero.


rdm.DeviceId.set_id_parts(manufacturer, device, subdevice)
==========================================================

Sets the manufacturer ID, device ID and subdevice ID for an existing DeviceId.

To address the root device, pass in a subdevice ID of 0.


rdm.DeviceId.set_id_whole(id)
=============================

Sets an existing DeviceId using a single 64-bit integer value, representing a combination of the manufacturer and device ID.

The Subdevice value will be set to zero.
