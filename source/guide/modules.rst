Module Scripts
##############

If the optional ``moduleMain`` property in the ``package.json`` file is specified, it refers to the module script. The module script is always run before instance scripts, and only once for each module. The global table resulting from running the module script is referred to as the module sandbox. Variables in the module sandbox are not visible to instance sandboxes, except by assigning them to the ``shared_table`` described below.

A module script may be necessary when dealing with finite controller resources. A set of instances may need to access the same UDP port; however, only one ``UdpSocket`` can be bound to a given UDP port. In this case, the ``UdpSocket`` object will have to be managed at the module level. This process is explained further by the examples in this section.

Module scripts are entirely optional. If they are not needed and add too much complexity to the module package, an ``instanceMain`` is all that is required. Module properties and the ``shared_table`` can be used without a module script.

Module API
==========

A ``module`` object is defined within the module sandbox. In addition to the properties of the ``module`` object exposed to module instances, this object exposes lifetime functions similar to those of module instances.

Module lifetime functions
=========================

You may define functions to run at key points in a module's lifetime. These mirror the lifetime functions of module instances.

* ``initialize()`` --- called right after the project loads, but before the ``initialize`` handlers of any module instances.
* ``net_up()`` --- called when the controller network interface comes up or just after the ``initialize`` handler if the network interface is up at the time of project unload. Guaranteed to be called before the ``net_up`` handlers of any module instances. May be called before or after the ``initialize`` handlers of any module instances.
* ``net_down()`` --- called when the controller network interface goes down or just before the ``cleanup`` handler if the network interface is up at the time of project unload. Called after the ``net_down`` handlers of any module instances. May be called before or after the ``cleanup`` handlers of any module instances.
* ``cleanup()`` --- called just before the project unloads. Called after the ``cleanup`` handlers of any module instances.

Module properties
=================

Properties defined in the ``moduleProperties`` field in the module configuration JSON file are set by the user in Designer for each module. To read the values of these properties in the [[[module script]]], use the ``module:property()`` method, passing the name of the property as defined in the configuration, e.g.

.. code-block:: lua

Shared table
============

The ``module`` object has a ``shared_table`` field. This field also appears in the ``module`` object exposed to the module instances. In this way, data managed at module scope can be exposed to its instances.

The ``shared_table`` is mutable. Changes applies to the ``shared_table`` in the module script will appear in all instances.

[[[example]]]