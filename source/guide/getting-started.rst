Getting started
###############

We'll work through the process of creating a simple timer module with start/stop actions and a timeout trigger. You can create a module package from scratch by creating the relevant text files within a folder on your computer but Designer has a tool to create the package definition & configuration files via a user interface, leaving you to add the Lua source code.

Create a module template
************************

To generate the core files of an IO module package, use the module template creator in Designer. This feature is currently in beta and must first be enabled from the Project Features tab in the Project view of Designer.

.. todo::

   **Screenshot:** project features with editor feature highlighted.

Once enabled, launch the IO module creator from the **Create...** button on the Modules toolbar in the Designer Trigger view.

.. todo::

   **Screenshot:** Module pane, highlighting Script & Modules button, Modules segmented control & Create... toolbar button.

Accept the default choice on the first page of the creator --- create a new module template.

Package properties
==================

The Package tab of the template creator lets you describe your module. When the module files are generated at the end of this process, these values will appear in the ``package.json`` file along with details of the files in your module.

Give the package a name, like *Simple Timer*, and type your name in the *Author* field.

Details of the other fields can be found in :ref:`module-anatomy-package`.

Icons
=====

Click **Import...** to browse for images you want to use as icons for triggers, conditions and actions exposed by your module.

*Note: you will soon be able to add an icon in-place when adding a trigger, condition or action in the Configuration tab.*

Configuration
=============

This is where you define how a user of Designer will interact with your module, so at this point we'll provide some suggestions to work towards a simple example.

We're going to build a timer module with start/stop actions and a timeout trigger.

With *Module* selected in the category list on the left, set the *Short name* property to *Timer*. Whatever you set as the short name will be prepended to any trigger, action or condition name in Designer. This gives you the option of not having to refer to your module name or function in every trigger, condition and action name. Leave this blank to ignore this feature.

.. todo::

   **Screenshot:** short name

We're going to make the interval of our timer a property of each instance, so if the user wants timers with different intervals they must create multiple instances of our module. Click **New Instance Property** on the toolbar and give the property the name *Interval*. We want to specify the interval in seconds, so set *Type* to *Integer* and choose *Spin box* as the *Editor*. We'll allow intervals up to 10 minutes, so set the range as 0 to 600. Type *seconds* (or just 's', or 'secs' --- really, you choose) in the *Suffix* editor so it's clear what the units are, then choose a default --- 5 seconds seems reasonable (0 would cause the timer to timeout on every playback refresh).

.. todo::

   **Screenshot:** instance property

Click **New Trigger** on the toolbar and change the name to *Timeout*. Pick an icon if you have one.

.. todo::

   **Screenshot:** trigger settings

Click **New Action** on the toolbar and change the name to *Start*. Click **New Action** again and set this second action's name to *Stop*.

.. todo::

   **Screenshot:** one of the actions

That's all we need to do for our template. Click **Next** to move on to the export stage. If you've missed anything out from the steps above you'll be shown the list of errors. Correct your mistakes and click **Next** to try again.

Export
======

Choose where you want your module template files to be created. The default should be fine unless you've already used the template creator, in which case you might want to adjust the folder name or its location.

Check the option to open the export location in a file browser and click **Export**.

Implement module functionality in Lua
*************************************

Open the file ``instance_main.lua`` in your favourite text editor from within your new module package folder. You'll see that Designer has already defined some handler functions for your trigger and the two actions.

Write module instance handlers
==============================

We want to create our timer and set its interval when an instance of our module is initialized. We'll do this inside the module instance ``initialize`` handler. Add the following:

.. code-block:: lua

   instance.initialize = function()
      -- create a new timer, which is 'global' within an instance of this module
      timer = iomodules.Timer.new()
      -- get instance "Interval" property (in seconds)
      local interval_secs = instance:property("Interval")
      -- convert to ms and set as timer interval
      timer.interval = interval_secs * 1000
   end

When the module instance is cleaned up, the instance ``cleanup`` handler is called. We should stop our timer if it's running:

.. code-block:: lua

   instance.cleanup = function()
      if timer.active then
         timer:stop()
      end
   end

Perform an action
=================

We need to add implementations for the action handlers that have been created in ``main.lua``

In the handler for the start action, add the following line to start the timer:

.. code-block:: lua

   timer:start()

And in the handler for the stop action, add the following line to stop the timer:

.. code-block:: lua

   timer:stop()

Fire a trigger
==============

When the timer's interval is reached we need to fire our module's *Timeout* trigger. We can hook up this behaviour in the ``initialize`` handler. Underneath where you set the timer interval, add the following:

.. code-block:: lua

   timer.timeout_handler = function()
      -- get the instance "Timeout" trigger and fire it
      instance:trigger("Timeout"):fire()
   end

At this point, your ``instance_main.lua`` file should have at least the following:

.. code-block:: lua

   instance.initialize = function()
       -- create a new timer, which is 'global' within an instance of this module
       timer = iomodules.Timer.new()
       -- get instance "Interval" property (in seconds)
       local interval_secs = instance:property("Interval")
       -- convert to ms and set as timer interval
       timer.interval = interval_secs * 1000
       timer.timeout_handler = function()
           -- get the instance "Timeout" trigger and fire it
           instance:trigger("Timeout"):fire()
       end
   end

   instance.cleanup = function()
       if timer.active then
           timer:stop()
       end
   end

   instance:action("Start").handler = function()
       timer:start()
   end

   instance:action("Stop").handler = function()
       timer:stop()
   end

Load module into project
************************

To try out your module in Designer, launch the IO module creator again from Modules toolbar in the Designer Trigger view, only this time choose the option to *Create from an existing package.json file*. Click **Next**.

.. todo::

   **Screenshot:** First page with second option selected

On the *Select Package File* page, set the file to be imported as the ``package.json`` file in your module package folder and click **Next**.

.. todo::

   **Screenshot:** Select Package File page

On the *Import / Export IO Module* page, you only need to have *Load module into project* selected.

.. todo:

   **Screenshot:** Import / Export IO Module page with only Load check box checked

Click **Finish** and your module will be added to your project, along with a new instance of the module. Notice the *Interval* property in the *Instance Properties*, set to the default of 5 seconds.

.. todo:

   **Screenshot:** module in module library, plus new instance properties (might want to wait for upcoming UI change)

Click the **New...** button on the Trigger toolbar --- you'll notice that the *Timer: Timeout* trigger from your module has been added to the list of available triggers.

Create a trigger of any type and click the **New...** button on the Action toolbar --- you'll see the *Timer: Start* and *Timer: Stop* actions are available.

Try it out! Timers work in Simulate in Designer.

Updating and correcting mistakes
********************************

If you want to make changes to your module source you can update existing modules in a Designer project. With the module selected in the module library, click **Update...** on the Module toolbar and navigate to your ``package.json`` file. Any triggers, conditions, actions and instance properties will be preserved within your project as far as possible after the update.
