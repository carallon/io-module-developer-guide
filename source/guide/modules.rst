Module scripts
##############

If the optional ``moduleMain`` property in the ``package.json`` file is specified, it refers to the *module script*. The module script is always run before instance scripts, and only once for each module. The global table resulting from running the module script is referred to as the *module sandbox*. Variables in the module sandbox are not visible to instance sandboxes, except by assigning them to the ``shared_table`` described below.

A module script may be necessary when dealing with finite controller resources. A set of instances may need to access the same UDP port; however, only one ``UdpSocket`` can be bound to a given UDP port. In this case, the ``UdpSocket`` object will have to be managed at the module level. This process is explained further by the examples in this section.

Module scripts are optional --- if they are not needed and add too much complexity to the module package, an ``instanceMain`` is all that is required. Module properties and the ``shared_table`` can be used without a module script.

Module API
==========

A ``module`` object is defined within the module sandbox. In addition to the properties of the ``module`` object exposed to module instances, this object exposes lifetime functions similar to those of module instances.

.. _module-package-information:

Package information
===================

A set of read-only properties provide the values of a subset of fields of the ``package.json`` file:

* ``name`` --- The name of the module, as specified by the ``"name"`` field of the ``package.json`` file.
* ``version`` --- The version of the module, as specified by the ``"version"`` field of the ``package.json`` file.
* ``apiVersion`` --- The IO module API version used by the module, as specified by the ``"ioModuleApiVersion"`` field of the ``package.json`` file.

Module lifetime functions
=========================

You may define functions to run at key points in a module's lifetime. These mirror the lifetime functions of module instances.

* ``initialize()`` --- called right after the project loads, but before the ``initialize`` handlers of any module instances.
* ``net_up()`` --- called when the controller network interface comes up or just after the ``initialize`` handler if the network interface is up at the time of project load. Guaranteed to be called before the ``net_up`` handlers of any of the IO module's instance sandboxes. Might be called before or after the ``initialize`` handlers of any module instances.
* ``net_down()`` --- called when the controller network interface goes down or just before the ``cleanup`` handler if the network interface is up at the time of project unload. Guaranteed to be called after the ``net_down`` handlers of any of the IO module's instance sandboxes. Might be called before or after the ``cleanup`` handlers of any module instances.
* ``cleanup()`` --- called just before the project unloads. Called after the ``cleanup`` handlers of any module instances.

Module properties
=================

Properties defined in the ``moduleProperties`` field in the module configuration JSON file are set by the user in Designer for each module. To read the values of these properties in the module script, use the ``module:property()`` method, passing the name of the property as defined in the configuration.

For example:

.. code-block:: lua

    local udp_socket = iomodules.net.UdpSocket.new()
    local inbound_port = module:property("Inbound Port")

    module.net_up = function()
        udp_socket:bind(inbound_port)
    end

Shared table
============

The ``module`` object has a ``shared_table`` field. This field also appears in the ``module`` object exposed to the module instances. In this way, data managed at module scope can be exposed to its instances.

The ``shared_table`` is mutable. Changes applied to the ``shared_table`` in the module script will appear in all instances.

As an example, consider an IO module which communicates with a particular type of device using the UDP protocol. Each device has a unique IP address. However, the devices send datagrams back to the controller on a single port. This port must be managed by the module itself. However, it cannot be known from the module script which instances correspond to which device. Thus we need a way for instances to inform the module script of their device's IP addresses:

.. code-block:: lua

    local inbound_socket = iomodules.net.UdpSocket.new()
    local inbound_port = module:property("Inbound Port")
    local datagram_received_callbacks = {}

    module.net_up = function()
        inbound_socket:bind(inbound_port)
    end

    module.net_down = function()
        inbound_socket:close()
    end

    module.shared_table.register_datagram_received_callback = function(ip_address, callback)
        datagram_received_callbacks[ip_address] = callback
    end

    inbound_socket.ready_read_handler = function(socket)
        local read_datagram_string_result = socket:read_datagram_string()
        -- Receive datagrams until there are none pending
        while read_datagram_string_result.valid do
            local sender_address = read_datagram_string_result.sender_address
            -- If there is a callback registered for the sender's IP address, call it
            local datagram_received_callback = datagram_received_callbacks[sender_address]
            if type(datagram_received_callback) == "function" then
                datagram_received_callback(read_datagram_string_result.data)
            end
            read_datagram_string_result = socket:read_datagram_string()
        end
    end

In ``shared_table`` we store a function, or closure, called ``register_datagram_received_callback`` which can be used by instances to associate their device's IP address with a callback. When the socket indicates that a datagram has arrived, the ``ready_read_handler`` determines the sender's IP address and calls the corresponding callback function with the payload data.

In the instance script, the following code is executed:

.. code-block:: lua

    local device_ip_address = instance:property("IP Address")
    local datagram_received_callback = function(data)
        -- use data
    end
    -- register callback with the device's IP address. When a datagram is received from the device,
    -- datagram_received_callback is called with the payload
    module.shared_table.register_datagram_received_callback(device_ip_address, datagram_received_callback)

Now instances will receive device messages directly without having to know about management of the underlying UDP socket.

.. _module-time-changes:

Time changes
============

Whenever the controller's local time changes, the ``time_change`` handler is called. The controller's new local time can be retrieved via ``controller.time.get_current_time()``:

.. code-block:: lua

    module.time_change = function()
        local dateTime = controller.time.get_current_time()
        controller.log("New time is " .. dateTime.utc_timestamp)
    end
