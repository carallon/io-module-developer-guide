Module packages
###############

The source code and resources of a module must be within a folder and its subfolders. We'll refer to this as the module *package*.

.. _module-anatomy-package:

Package definition
******************

Details of the module package are defined by a ``package.json`` file containing a single JSON object.

For example:

.. code-block:: json

   {
       "name": "Example module",
       "description": "A demonstration of an IO module package",
       "version": "1.0",
       "uuid": "{84bea226-4123-4cc5-be3c-c8adee049e9c}",
       "ioModuleApiVersion": "2.0",
       "controllerApiVersion": 2,
       "author": "Inspired Coding",
       "contributors": [
           {
               "name":"Lady Mary",
               "email":"lady.mary@inspiredcoding.com",
               "url":"https://www.inspiredcoding.com"
           },
           {
               "name":"John Smith"
           }
       ],
       "config": "config.json",
       "instanceMain": "instance.lua",
       "moduleMain": "module.lua",
       "sources": [
           "lib/example.lua"
       ]
   }

Full details of the members of the package object are as follows:

.. list-table::
   :widths: 2 1 1 4
   :header-rows: 1

   * - Name
     - Type
     - Required
     - Description
   * - ``name``
     - string
     - Yes
     - Name of the module - will be displayed in Designer in the module library.
   * - ``description``
     - string
     - No
     - A short summary of the module.
   * - ``version``
     - string
     - Yes
     - The version of the module, in the format ``a.b`` or ``a.b.c``.
   * - ``uuid``
     - string
     - Yes
     - Unique ID of the module. You'll need to generate one if writing a module from scratch. Must be in the format ``xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`` where each ``x`` is a hexadecimal digit. It is customary to enclose the UUID value in curly braces: ``{}`` but this is optional.
   * - ``ioModuleApiVersion``
     - string
     - Yes
     - The IO module API version required to run this module. Must be in the format ``a.b`` or ``a.b.c``. The latest version at time of writing is ``2.0``.
   * - ``controllerApiVersion``
     - number
     - Yes
     - The controller API version required to run this module. Must be one of the released Designer Controller API versions.
   * - ``author``
     - string
     - Yes
     - The name of the company or individual who wrote the module.
   * - ``contributors``
     - array
     - No
     - Array of people who contributed to the module (see :ref:`module-contributor`).
   * - ``config``
     - string
     - Yes
     - Relative path of the configuration JSON file.
   * - ``instanceMain``
     - string
     - Yes
     - Relative path of the Lua file executed for each module instance.
   * - ``moduleMain``
     - string
     - No
     - Relative path of the Lua file executed before any of a module's instances are initialised.
   * - ``sources``
     - array
     - No
     - Array of additional Lua source files, which may be required from the main Lua file or from other source files in the module.

.. _module-contributor:

Module contributor
==================

A contributor in the ``contributors`` array in the ``package.json`` file is a JSON object    with the following members:

.. list-table::
   :widths: 1 1 1 5
   :header-rows: 1

   * - Name
     - Type
     - Required
     - Description
   * - ``name``
     - string
     - Yes
     - Name of the contributor.
   * - ``email``
     - string
     - No
     - Email address of the contributor.
   * - ``url``
     - string
     - No
     - A url relating to the contributor, e.g. a personal website or social media profile.

Configuration
*************

The configuration JSON file within the module package defines the user interface elements added to Designer when the module is imported into a project, including:

* Triggers
* Conditions
* Actions
* Module properties
* Module instance properties
* Module instance status variables

For example:

.. code-block:: json

   {
       "shortName":"Projector",
       "triggers": [
           {
               "name": "Connected",
               "icon": "icons/connected.svg"
           }
       ],
       "conditions": [
           {
               "name": "Lamp State",
               "icon": "icons/lamp_state.svg",
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
       ],
       "actions": [
           {
               "icon": "icons/lamp_control.svg",
               "name": "Lamp Control",
               "properties": [
                   {
                       "name": "State",
                       "type": "bool",
                       "editor": {
                           "type": "dropdown",
                           "items": [
                               {
                                   "text": "On",
                                   "value": true
                               },
                               {
                                   "text": "Off",
                                   "value": false
                               }
                           ],
                           "default": 1
                       }
                   }
               ]
           }
       ],
       "instanceStatusVariables": [
           {
               "key": "lampState",
               "label": "Lamp State",
               "type": "string"
           }
       ],
       "instanceProperties": [
           {
               "name": "IP address",
               "type": "ipAddress",
               "editor": {
                   "type": "ipAddress",
                   "default": "0.0.0.0"
               }
           }
       ]
   }

Full details of the members of the configuration object are as follows:

.. list-table::
   :widths: 1 1 1 5
   :header-rows: 1

   * - Name
     - Type
     - Required
     - Description
   * - ``shortName``
     - string
     - No
     - If provided, this string will prepended to the names of all triggers, conditions and actions defined by the module when displayed in Designer. In the example above, the trigger named "Connected" will be shown in Designer as "Projector: Connected". In Lua source, you can still refer to the trigger as "Connected".
   * - ``triggers``
     - array
     - No
     - An array of trigger objects (see below).
   * - ``conditions``
     - array
     - No
     - An array of condition objects (see below).
   * - ``actions``
     - array
     - No
     - An array of action objects (see below).
   * - ``instanceStatusVariables``
     - array
     - No
     - An array of status variables. This can have at most 50 entries.
   * - ``instanceProperties``
     - array
     - No
     - An array of user properties to be set for each module instance (see :ref:`user-property`).
   * - ``moduleProperties``
     - array
     - No
     - An array of user properties common to every module instance.

.. _status-variable-package-information:

Status Variable
===============

The ``instanceStatusVariables`` array in the configuration JSON object comprises objects with the following members:

.. list-table::
   :widths: 1 1 1 5
   :header-rows: 1

   * - Name
     - Type
     - Required
     - Description
   * - ``key``
     - string
     - Yes
     - The key of the status variable when accessed from instance scripts. This can only contain characters in the set ``[a-zA-Z0-9]``, and may not exceed 20 characters in length. Every ``key`` must be unique.
   * - ``label``
     - string
     - No
     - The name of the status variable when displayed outside of instance scripts. This may not exceed 120 characters in length. If not specified, ``key`` will be used as the ``label``. Every ``label`` must be unique.
   * - ``type``
     - string
     - No
     - The ``type`` of the status variable. Currently only the type ``"string"`` is supported. If this property is not specified, ``"string"`` is the default type.

Trigger/Condition/Action object
===============================

The ``triggers``, ``conditions`` and ``actions`` arrays in the configuration JSON object comprise objects with the following members:

.. list-table::
   :widths: 1 1 1 5
   :header-rows: 1

   * - Name
     - Type
     - Required
     - Description
   * - ``name``
     - string
     - Yes
     - User-friendly name (will be prepended with configuration ``shortName``, if provided).
   * - ``aka``
     - array
     - No
     - An array of strings, in addition to ``name``, that will be used to uniquely match this trigger/condition/action when updating the module from source.
   * - ``icon``
     - string
     - No
     - Relative path to an image in the module package to be used as an icon for this trigger/condition/action in Designer and on the controller web interface.
   * - ``properties``
     - array
     - No
     - An array of user properties to be exposed in the Designer (see :ref:`user-property`).

.. _user-property:

User property
=============

The ``properties`` array in the configuration JSON object and the trigger/condition/action JSON object comprise objects with the following members:

.. list-table::
   :widths: 1 1 1 5
   :header-rows: 1

   * - Name
     - Type
     - Required
     - Description
   * - ``name``
     - string
     - Yes
     - User-friendly name that will be displayed next to the property editor in Designer.
   * - ``aka``
     - array
     - No
     - An array of strings, in addition to ``name``, that will be used to uniquely match this property when updating the module from source.
   * - ``type``
     - string
     - Yes
     - Value type of the property. Supported basic types are: ``int``, ``string``, ``double``, ``bool`` & ``ipAddress``. Supported resource types are: ``digitalIn``, ``digitalOut``, ``analogIn`` & ``serial`` (see :ref:`resource-property-types`).
   * - ``editor``
     - object
     - No
     - An editor object for the given ``type`` (see :ref:`user-property-editor`). Must not be specified for resource types.
   * - ``variablesEnabled``
     - bool
     - No
     - Whether this property may be set from a trigger variable. Only applies to action and condition (since Designer 2.12.1) properties. Default is true if not specified.

.. _user-property-editor:

User property editor
--------------------

The ``editor`` object in user properties always has the following ``type`` member:

.. list-table::
   :widths: 1 1 1 5
   :header-rows: 1

   * - Name
     - Type
     - Required
     - Description
   * - ``type``
     - string
     - Yes
     - Type of the user interface control to be used for the editor. Supported types are: ``dropdown``, ``spinbox``, ``doubleSpinbox``, ``ipAddress``, ``toggle`` & ``lineEdit``.

Depending on the ``type``, the ``editor`` object has additional members as detailed in the following sections.

Drop down editor
^^^^^^^^^^^^^^^^

An editor of type ``dropdown`` has the following members:

.. list-table::
   :widths: 1 1 1 5
   :header-rows: 1

   * - Name
     - Type
     - Required
     - Description
   * - ``items``
     - array
     - Yes
     - Items to populate the drop down editor (see below).
   * - ``default``
     - number
     - No
     - Index into the ``items`` array to use as the default value for new instances of the property. The index is 1-based, like Lua table.

The ``items`` array of a ``dropdown`` editor comprises objects with the following members:

.. list-table::
   :widths: 1 3 3 3
   :header-rows: 1

   * - Name
     - Type
     - Required
     - Description
   * - ``text``
     - string
     - Yes
     - User-friendly text that will be displayed in the drop down editor for this item.
   * - ``value``
     - Must match the ``type`` of the parent property.
     - Yes, unless the parent property is of ``type`` string.
     - Value that will be set on the property when this item is chosen by the user.

.. _spin-box-editor:

Spin box editor
^^^^^^^^^^^^^^^

An editor of type ``spinbox`` has the following members:

.. list-table::
   :widths: 1 1 1 5
   :header-rows: 1

   * - Name
     - Type
     - Required
     - Description
   * - ``min``
     - number
     - No
     - Minimum value of the property; default is 0.
   * - ``max``
     - number
     - No
     - Maximum value of the property; default is 255.
   * - ``step``
     - number
     - No
     - Increment/decrement for up/down arrows of the spin box editor; default is 1.
   * - ``suffix``
     - string
     - No
     - Text to display after the value in the editor.
   * - ``default``
     - number
     - No
     - Default value of the property for new instances; default is 0.
   * - ``specialValueText``
     - string
     - No
     - Text to display in the property editor instead of a numeric value when the current value is equal to ``min``. The value of the property in the Lua scripts will still be ``min``.
   * - ``base``
     - integer
     - No
     - The numerical base in which to display the value, e.g. Base-16 would be Hex; default is 10.


.. _double-spin-box-editor:

Double spin box editor
^^^^^^^^^^^^^^^^^^^^^^

An editor of type ``doubleSpinbox`` has the following members:

.. list-table::
   :widths: 1 1 1 5
   :header-rows: 1

   * - Name
     - Type
     - Required
     - Description
   * - ``min``
     - number
     - No
     - Minimum value of the property; default is 0.0.
   * - ``max``
     - number
     - No
     - Maximum value of the property; default is 1.0.
   * - ``step``
     - number
     - No
     - Increment/decrement for up/down arrows of the spin box editor; default is 0.1.
   * - ``suffix``
     - string
     - No
     - Text to display after the value in the editor.
   * - ``decimals``
     - number
     - No
     - Precision of the spin box - how many decimals the editor will use to display and interpret values; default is 2.
   * - ``default``
     - number
     - No
     - Default value of the property for new instances; default is 0.0.
   * - ``specialValueText``
     - string
     - No
     - Text to display in the property editor instead of a numeric value when the current value is equal to ``min``. The value of the property in the Lua scripts will still be ``min``.

IP address editor
^^^^^^^^^^^^^^^^^

An editor of type ``ipAddress`` has the following members:

.. list-table::
   :widths: 1 1 1 5
   :header-rows: 1

   * - Name
     - Type
     - Required
     - Description
   * - ``default``
     - string
     - No
     - Default IP address for new instances; editor will be blank (invalid) if default not specified.

Toggle editor
^^^^^^^^^^^^^

An editor of type ``toggle`` (a check box) has the following members:

.. list-table::
   :widths: 1 1 1 5
   :header-rows: 1

   * - Name
     - Type
     - Required
     - Description
   * - ``default``
     - bool
     - No
     - Default state of the check box for new instances; default is false (unchecked).

Line editor
^^^^^^^^^^^

An editor of type ``lineEdit`` has the following members:

.. list-table::
   :widths: 1 1 1 5
   :header-rows: 1

   * - Name
     - Type
     - Required
     - Description
   * - ``placeholder``
     - string
     - No
     - Placeholder text for the editor when its string value is empty.
   * - ``validator``
     - string
     - No
     - Regular expression to match for a valid user input. See the note on regular expressions below.
   * - ``default``
     - string
     - No
     - Default string value of the property for new instances.

Regular expressions
"""""""""""""""""""

Good references about regular expressions include the `Perl regular expression documentation <http://perldoc.perl.org/perlre.html>`_ and the `Perl regular expression tutorial <http://perldoc.perl.org/perlretut.html>`_.

Note that you must escape all backslashes in a pattern string with another backslash. For example, to match two digits followed by a space and a word:

.. code-block:: json

   "validator": "\\d\\d \\w+"

.. _resource-property-types:

Resource property types
-----------------------

Where IO modules need to interact with finite resources on the controller, such as a serial port, the property ``type`` should be set to a special resource type. This allows Designer to track the use of these resources to avoid conflicts with other modules and with other triggers and actions.

Resource types do not support custom editors - a standard editor will be presented to the user for these property types.

Digital input
^^^^^^^^^^^^^

Set user property ``type`` to value ``digitalIn``.

:doc:`Digital inputs <../api/gpio/digitalinput>` are available on controllers and remote devices. To listen for changes in the state of a digital input, you must have at least one of these properties in the configuration.

Digital output
^^^^^^^^^^^^^^

Set user property ``type`` to value ``digitalOut``.

:doc:`Digital outputs <../api/gpio/digitaloutput>` are available on some remote devices. To control the state of a digital output, you must have at least one of these properties in the configuration.

Analog input
^^^^^^^^^^^^

Set user property ``type`` to value ``analogIn``.

:doc:`Analog inputs <../api/gpio/analoginput>` are available on controllers and remote devices. To listen for changes in the level of an analog input, you must have at least one of these properties in the configuration.

Serial interface
^^^^^^^^^^^^^^^^

Set user property ``type`` to value ``serial``.

:doc:`Serial <../api/serial>` interfaces are available on controllers and remote devices. To send/receive data on a serial interface in an IO module, you must have at least one of these properties in the configuration.

Icons
*****

Image files referenced by the configuration JSON file for module triggers, conditions and actions need to be included in the module package. They may be in a subfolder, as long as the configuration JSON file has the correct relative paths.

We recommend using SVG images for your module icons - they scale well for use on high DPI monitors. IO modules also support PNG and JPEG images as icons --- we recommend you generate them with dimension 64 pixels.

Lua sources
***********

IO module functionality is written in Lua. Two specified Lua sources act as entry points. The ``instanceMain`` file is executed for each module instance, and is a required part of the package. The optional ``moduleMain`` file is executed for each module and can manage data common to all instances of a module. These files may ``require`` other Lua files within the module package. All Lua files included through ``require`` must be listed in the ``sources`` array in ``package.json``.

For example, in ``package.json``, you might have:

.. code-block:: json

   {
      "moduleMain": "module_main.lua",
      "instanceMain": "instance_main.lua",
      "sources": [
         "lib/example.lua"
      ]
   ...


Then in ``instance_main.lua`` or ``module_main.lua`` you can write:

.. code-block:: lua

   require('example')

The string passed to ``require`` may be a file base name, as shown above, or a path to a Lua file within the module package, e.g. ``lib/example.lua``.

Documentation
*************

A module may include a single ``readme.md`` file in the same folder as ``package.json``, written in `CommonMark <http://commonmark.org/>`_. This will be displayed to the user in the module management user interface in Designer.
