Property
########

Expanded ``Instance`` and ``Module`` property functionality.

Properties
**********

Property.value -> Any
=====================

Read only. The value of the property.

.. code-block:: lua

    local ip_addr = instance:property_object("IP Address").value

Event handlers
**************

Property.is_enabled
=====================

.. note::
    This function is only used by Designer during design time.
    It is not utilised during run-time on a Controller.

The handler is evaluated by Designer when a module is loaded, or any property is altered.
Returns ``true`` or ``false`` to indicate if the named property should be enabled or disabled.
Disabled properties are grayed out and can not be altered by the user.

If the function is omitted, then the property will remain enabled.

.. code-block:: lua

    instance:property_object("IP Address").is_enabled = function()
        if (someTestCondition) then
            return true -- Property "IP Address" is enabled in Designer
        else
            return false -- Property "IP Address" is disabled in Designer
        end
    end

Property.is_visible
=====================

.. note::
    This function is only used by Designer during design time.
    It is not utilised during run-time on a Controller.

The handler is evaluated by Designer when a module is loaded, or any property is altered.
Returns ``true`` or ``false`` to indicates if the named property should be visible or hidden.

If the function is omitted, then the property will remain visible.

.. code-block:: lua

    instance:property_object("IP Address").is_visible = function()
        if (someTestCondition) then
            return true -- Property "IP Address" is visible in Designer
        else
            return false -- Property "IP Address" is hidden in Designer
        end
    end
