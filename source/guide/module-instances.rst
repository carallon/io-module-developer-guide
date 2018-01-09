Module instances
################

For a module to be used in a Designer project, at least one *instance* of the module must be created by the user. For a simple module, like a timer, creating more instances gives the user a set of independent timers. For a more complex module, such as an integration with a projector, each instance will connect to a different projector and control it independently of the others.

The user must choose an instance when using the module's triggers, conditions and actions, although if there is only one instance of the module in their project then this will be selected by default. For triggers, the instance setting defines which instance must fire the trigger for it to match. For conditions, the instance setting defines which instance will test the condition. For actions, the instance setting defines which instance will perform the action.

When writing a module, you are writing for one instance of the module; all Lua code for a module runs in an instance sandbox. Any Lua variables you create at 'global' scope in module code are not visible to other modules --- they are only global in the sense that they will exist throughout the life of an instance of the module.

Module instances only run on the Primary controller in a project with the exception of :ref:`broadcast events <broadcast-event>`, which run on all controllers.

Instance API
************

The ``module`` object is defined within the instance sandbox. This allows you to interact with the triggers, conditions and actions defined in the module configuration, to access the instance properties, and to implement handler functions for each instance.

Module name
===========

The user can rename a module instance in Designer. This name may be read in Lua from the ``module.name`` property.

Instance lifetime functions
===========================

You may define functions to run at key points in a module instance's lifetime.

Module instances are created when a controller loads a project and cleaned up when a project unloads. The ``initialize`` handler of each instance is called right after a project loads. The ``cleanup`` handler is called just before a project unloads.

For example:

.. code-block:: lua

    module.initialize = function()
        -- create a timer for use in the module
        myTimer = iomodules.Timer.new()
        myTimer.interval = 10000
    end

    module.cleanup = function()
        -- stop the timer, if still running
        myTimer:stop()
    end

Modules that use networking need to know when the controller's network interface is up. They should provide implementations for the following handlers:

* ``net_up()`` --- called when the controller network interface comes up or just after the ``initialize`` handler (after project load) if the network interface is already up at this time.
* ``net_down()`` --- called when the controller network interface goes down or just before the ``cleanup`` handler (before project unload) if the network interface is up at this time.

For example:

.. code-block:: lua

    module.net_up = function()
        -- (re-)establish a connection
        socket:connect(ip_addr, port, iomodules.Stream.READ_ONLY_MODE)
    end

    module.net_down = function()
        socket:disconnect()
    end

Instance properties
===================

Properties defined in the module configuration JSON file are set by the user in Designer for each module instance. To read the value of these properties for the instance in Lua, use the ``module:property()`` method, passing the name of the property as defined in the configuration, e.g.

.. code-block:: lua

    local ip_addr = module:property("IP Address")

Triggers
========

Triggers defined in the module configuration JSON file are accessed in Lua through the ``module:trigger()`` method, which takes the name of the trigger as defined in the configuration as its only argument. For example:

.. code-block:: lua

    local state_changed_trigger = module:trigger("State Changed")

Trigger objects have a ``fire()`` method to queue a trigger event for the next playback refresh. You may pass one argument to this method, which will be passed on to the trigger's ``test`` handler.

When a trigger is fired, a controller will work through the list of triggers defined in the project in order, looking for a trigger that is both of the same type and has matching properties. There may be multiple triggers of the same type in a project, each looking for different criteria to match. To determine whether the circumstances that caused the trigger to fire are a match for a trigger in the project, you implement the trigger's ``test`` handler and return ``true`` for a match. If you don't implement a ``test`` handler for a trigger then any trigger of its type in the project will match, as if you had implemented a function that always returned ``true``. The ``test`` handler is passed 3 arguments:

* ``data`` --- the match data passed to the trigger's ``fire()`` method.
* ``properties`` --- name/value pairs (string-indexed Lua table) of the trigger properties set by the user, as defined in the module configuration.
* ``variables`` --- this integer-indexed Lua table may be modified to set *variables* for use in any actions attached to this trigger in the project. See the Designer help for information about capturing variables in triggers.

You must return from the ``test`` handler as soon as possible else you risk reducing the refresh rate of the controller's playback engine.

For example, if you have defined a trigger in your module configuration:

.. code-block:: json

    {
        "triggers": [
            {
                "name": "State Changed",
                "icon": "icons/triggers/state_changed.svg",
                "properties": [
                    {
                        "name": "State",
                        "type": "int",
                        "editor": {
                            "type": "dropdown",
                            "items": [
                                {
                                    "text": "Off",
                                    "value": 0
                                },
                                {
                                    "text": "On",
                                    "value": 1
                                },
                                {
                                    "text": "Blown",
                                    "value": 2
                                }
                            ],
                            "default": 2
                        }
                    }
                ]
            }
    ...

Then you would want to pass a number for the state to ``fire()`` so you can match this information in the ``test`` handler:

.. code-block:: lua

    -- fire the trigger to announce the state has changed to value 1 (On)
    function fireStateOnTrigger()
        module:trigger("State Changed"):fire(1)
    end

    -- define the test handler for the State Changed trigger
    module:trigger("State Changed").test = function(data, properties, variables)
        -- get the state set by the user in the trigger properties - will be the value, not the text
        local statePropertyValue = properties["State"]
        if statePropertyValue == data then
            -- push state onto variables
            table.insert(variables, data)
            -- match
            return true
        end
        -- don't match this trigger
        return false
    end

To determine the string describing the trigger that will be displayed in the Designer Trigger UI and on the controller's web interface, you should set the trigger's :ref:`description_handler <description-handler>`.

Conditions
==========

Conditions defined in the module configuration JSON file are accessed in Lua through the ``module:condition()`` method, which takes the name of the condition as defined in the configuration as its only argument. For example:

.. code-block:: lua

    local connected_condition = module:condition("Connected")

To test the condition, you implement the condition's ``handler`` function, returning ``true`` if the condition is met. If you don't implement the condition ``handler``, the condition will always fail, as if you'd implemented a handler that always returns ``false``.

The ``handler`` function is passed 2 arguments:

* ``properties`` --- name/value pairs (string-indexed Lua table) of the condition properties set by the user, as defined in the module configuration.
* ``variables`` --- the variables captured by the trigger as an integer-indexed Lua table. Each variable is of the type ``Variant``. See the `Scripting API documentation <http://www.pharoscontrols.com/software_help/designer2/Default.htm#Help/Reference/Scripting/Variants.htm>`_ for information about Variants. See the Designer help for information about using variables in conditions.

You must return from the ``handler`` function as soon as possible else you risk reducing the refresh rate of the controller's playback engine.

For example, if you have defined a condition in your module configuration:

.. code-block:: json

    {
        "conditions": [
            {
                "name": "Connected",
                "icon": "icons/connected.svg",
                "properties": [
                    {
                        "name": "Connected",
                        "type": "bool",
                        "editor": {
                            "type": "dropdown",
                            "items": [
                                {
                                    "text": "No",
                                    "value": false
                                },
                                {
                                    "text": "Yes",
                                    "value": true
                                }
                            ],
                            "default": 1
                        }
                    }
                ]
            }
    ...

Then you could define the handler as follows:

.. code-block:: lua

    module:condition("Connected").handler = function(properties, variables)
        -- get boolean value of user property
        local connectedProperty = properties["Connected"]
        -- compare against some cached state for the instance
        return connectedProperty == isConnected
    end

To determine the string describing the condition that will be displayed in the Designer Trigger UI and on the controller's web interface, you should set the condition's :ref:`description_handler <description-handler>`.

Actions
=======

Actions defined in the module configuration JSON file are accessed in Lua through the ``module:action()`` method, which takes the name of the action as defined in the configuration as its only argument. For example:

.. code-block:: lua

    local lamp_on_action = module:action("Lamp On")

To implement a function to perform the action, you implement the action's ``handler`` function, which is passed 2 arguments:

* ``properties`` --- name/value pairs (string-indexed Lua table) of the action properties set by the user, as defined in the module configuration.
* ``variables`` --- the variables captured by the trigger as an integer-indexed Lua table. Each variable is of the type ``Variant``. See the `Scripting API documentation <http://www.pharoscontrols.com/software_help/designer2/Default.htm#Help/Reference/Scripting/Variants.htm>`_ for information about Variants. See the Designer help for information about using variables in actions.

You must return from the ``handler`` function as soon as possible else you risk reducing the refresh rate of the controller's playback engine.

For example, if you have defined an action in your module configuration:

.. code-block:: json

    {
        "actions": [
            {
                "name": "Lamp On",
                "icon": "icons/lamp_on.svg"
            }
    ...

Then you could define the handler as follows:

.. code-block:: lua

    module:action("Lamp On").handler = function(properties, variables)
        projector:send_lamp_on()
    end

To determine the string describing the action that will be displayed in the Designer Trigger UI and on the controller's web interface, you should set the action's :ref:`description_handler <description-handler>`.

.. _description-handler:

Description handler
===================

The Designer Trigger UI and the controller's web interface display descriptive strings about triggers, conditions and actions, reflecting the property values set by the user. To determine this string for the triggers, conditions and actions in a module, you should assign a function to the ``description_handler`` property. The ``description_handler`` is passed a table with the property values, keyed with the property names. For conditions, the ``description_handler`` is passed an additional parameter, ``negate``, which is true if the condition is negated and false otherwise.

.. note:: Properties being set from trigger variables will have a string value of "<variable x>", where x is the variable number set by the user.

For example:

.. code-block:: lua

    module:action("Set Mode").description_handler = function(properties)
        return "Set mode "..properties.Mode
    end


.. _broadcast-event:

Broadcast
=========

Module instances only run on the Primary controller of a project. Sometimes it's necessary to run some Lua code on all controllers, particularly when calling the :doc:`./controller-api`, which only affects the local controller.

You initiate a broadcast with the ``module:broadcast()`` method, which takes a single, optional `array <http://www.lua.org/pil/11.1.html>`_ (integer-indexed table) argument. The values of the array may be strings or numbers only.

Set a function on the instance's ``broadcast_event`` property to handle broadcasts. This ``broadcast_event`` will run on *all* controllers in a project. It will receive an array with the same values as were passed to the ``module:broadcast()`` method (but not the *same* array - the data will have been sent across the network to other controllers).

For example, to start timeline 4 on all controllers from a module action:

.. code-block:: lua

    module:action("Broadcast Example").handler = function(properties, variables)
        local timelineNum = 4
        module:broadcast(timelineNum)
    end

    module.broadcast_event = function(variables)
        controller.log("Action: Broadcast Example - Timeline "..variables[1].." is playing next")
        controller.get_timeline(variables[1]):start()
    end

Where ``variables`` is the array of values passed to ``module:broadcast()``, converted to the ``Variant`` type. See the `Scripting API documentation <http://www.pharoscontrols.com/software_help/designer2/Default.htm#Help/Reference/Scripting/Variants.htm>`_ for information about Variants.
